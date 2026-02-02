# ARCHITECTURE.md Schema

This document defines the structure and versioning of ARCHITECTURE.md files managed by the arch plugin.

## Schema Version: 1.0.0

### File Structure

Every ARCHITECTURE.md file managed by this plugin MUST include frontmatter and follow this structure:

```yaml
---
managed-by: arch-plugin
schema-version: 1.0.0
doc-version: 1.0.0
last-updated: YYYY-MM-DD
---

# [Component Name]

## Purpose
[Single paragraph: What this component does and why it exists]

## External Interfaces

### Provides (Ports)
[What this component exposes to other components or external systems]
- `function_name(params: type) -> return_type`: Description
- `ClassName`: Exported class/type with brief purpose

### Requires (Ports)
[What this component depends on from other components]
- `DependencyComponent.function_name(params: type) -> return_type`: How it's used
- External dependencies (libraries, APIs, etc.)

## Internal Architecture

### Subcomponents
[List of child components, if any]
- **SubcomponentName** (at `./path/`): Brief purpose

### Internal Interfaces (Connections)
[How subcomponents interact with each other]
- SubA → SubB: `interface_name()` - Purpose of connection
- SubB → SubC: Data flow description

## Notes
[Optional: Design decisions, constraints, future considerations]

---
*Parent: [../ParentComponent or "root"]*
```

## Directory Structure

The progressive decomposition pattern uses a **modular file structure** where each component directory contains its own ARCHITECTURE.md:

```
project/
├── ARCHITECTURE.md              # Root - always exists
├── auth/
│   ├── ARCHITECTURE.md          # Auth component
│   ├── session/
│   │   └── ARCHITECTURE.md      # Session subcomponent
│   └── tokens.ts                # Single-file (described in auth/ARCHITECTURE.md)
├── api/
│   └── ARCHITECTURE.md          # API component
└── database/
    └── ARCHITECTURE.md          # Database component
```

### When to Create a Component Directory

**CREATE a new component directory + ARCHITECTURE.md when:**
- New functionality has a distinct, cohesive purpose
- The component will have multiple files (>2-3 related files)
- It represents a logical domain boundary (auth, api, database, payments)
- It will be reused by multiple other components
- It needs its own clear external interface

**DO NOT create a component directory when:**
- It's a single utility file → describe in parent's Internal Architecture
- It's a helper class for an existing component → describe in parent
- It duplicates or overlaps significantly with existing components
- The functionality is a simple implementation detail

### Component Creation Checklist

When adding a new component:
1. Create the directory: `mkdir -p ./path/to/component`
2. Create `./path/to/component/ARCHITECTURE.md` with proper frontmatter
3. Populate all required sections (Purpose, External Interfaces, Internal Architecture)
4. Add parent reference at the bottom: `*Parent: ../ParentComponent*`
5. Update parent's ARCHITECTURE.md to list the new subcomponent
6. Verify bidirectional references are correct

### Depth Guidelines

- **Level 1 (root)**: Major system components (auth, api, database, ui)
- **Level 2**: Subcomponents within major components (auth/session, auth/oauth)
- **Level 3**: Complex subcomponents that warrant their own docs
- **Beyond**: Rarely needed; stop at 3 levels unless clearly justified

## Frontmatter Fields

### Required Fields

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `managed-by` | string | Must be "arch-plugin" to indicate plugin management | `arch-plugin` |
| `schema-version` | semver | Version of this schema definition | `1.0.0` |
| `doc-version` | semver | Version of this document's content | `2.1.3` |
| `last-updated` | date | ISO date of last modification | `2026-02-01` |

### Optional Fields

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `authors` | array | Contributors to this component | `["alice", "bob"]` |
| `deprecated` | boolean | Whether component is deprecated | `true` |
| `stability` | string | Stability level: experimental, stable, frozen | `stable` |

## Document Versioning

### doc-version Semantics

Following semantic versioning (MAJOR.MINOR.PATCH):

**MAJOR** version increment when:
- Component purpose fundamentally changes
- Breaking changes to external interfaces (signature changes, removals)
- Component is split or merged
- Major restructuring of subcomponents

**MINOR** version increment when:
- New external interfaces added (backwards-compatible)
- New subcomponents added
- New dependencies added
- Significant expansion of internal architecture

**PATCH** version increment when:
- Documentation clarifications or typo fixes
- Notes or examples added
- Internal implementation details updated
- No interface or structural changes

### Schema Versioning

The `schema-version` field tracks the version of THIS document's specification.

**Current: 1.0.0**

Schema changes:
- **MAJOR**: Incompatible structure changes (different sections, removed required fields)
- **MINOR**: New optional fields or sections
- **PATCH**: Clarifications to schema documentation

## Validation Rules

Files with `managed-by: arch-plugin` frontmatter MUST:

1. Include all required frontmatter fields
2. Have valid semver for both `schema-version` and `doc-version`
3. Include all required sections: Purpose, External Interfaces, Internal Architecture
4. Have valid parent reference at bottom
5. Use consistent formatting for interface signatures

## Migration Guide

### Unmanaged → Managed

To bring an existing ARCHITECTURE.md under plugin management:

1. Add frontmatter with `managed-by: arch-plugin`
2. Set `schema-version: 1.0.0`
3. Set `doc-version: 1.0.0` (starting point)
4. Ensure structure matches schema
5. Run `/arch:query Check architecture consistency` to validate

### Schema Version Upgrades

When schema version changes, the plugin will:

1. Detect outdated `schema-version` on file read
2. Offer to migrate to new schema
3. Preserve content while updating structure
4. Increment `doc-version` minor version
5. Update `last-updated` timestamp

## Examples

### Minimal Component (Single-file)

```yaml
---
managed-by: arch-plugin
schema-version: 1.0.0
doc-version: 1.0.0
last-updated: 2026-02-01
---

# Utilities

## Purpose
Common utility functions used across the application.

## External Interfaces

### Provides (Ports)
- `formatDate(date: Date) -> string`: Format dates consistently
- `deepClone<T>(obj: T) -> T`: Deep clone objects

### Requires (Ports)
None

## Internal Architecture
Single-file implementation (./utils.ts)

---
*Parent: root*
```

### Complex Component (Multi-level)

```yaml
---
managed-by: arch-plugin
schema-version: 1.0.0
doc-version: 2.3.1
last-updated: 2026-02-01
authors: ["alice", "bob"]
stability: stable
---

# Authentication System

## Purpose
Manages user authentication, session management, and authorization across the application.

## External Interfaces

### Provides (Ports)
- `login(credentials: Credentials) -> Promise<Session>`: Authenticate user
- `logout(sessionId: string) -> Promise<void>`: End user session
- `checkPermission(userId: string, resource: string) -> Promise<boolean>`: Check authorization
- `Session`: Type representing active user session

### Requires (Ports)
- `Database.findUser(email: string) -> Promise<User>`: User lookup
- `Database.saveSession(session: Session) -> Promise<void>`: Persist session
- `CryptoService.hashPassword(password: string) -> string`: Password hashing
- `CryptoService.verifyPassword(password: string, hash: string) -> boolean`: Password verification

## Internal Architecture

### Subcomponents
- **SessionManager** (at `./session/`): Session lifecycle and storage
- **PermissionChecker** (at `./permissions/`): Authorization rules and checks
- **OAuth2Provider** (at `./oauth/`): OAuth2 integration for external providers

### Internal Interfaces (Connections)
- SessionManager → PermissionChecker: `getSessionPermissions(sessionId)` - Load permissions for active session
- OAuth2Provider → SessionManager: `createSessionFromOAuth(token)` - Create session from OAuth token

## Notes
- Sessions expire after 24 hours of inactivity
- OAuth2 currently supports Google and GitHub providers
- Permission system uses role-based access control (RBAC)
- Future: Add support for multi-factor authentication

---
*Parent: root*
```

## Change Log

### 1.0.0 (2026-02-01)
- Initial schema definition
- Core frontmatter fields
- Standard document structure
- Versioning semantics
