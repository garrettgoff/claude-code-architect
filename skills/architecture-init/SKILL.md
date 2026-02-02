---
name: architecture-init
description: Bootstrap the progressive decomposition architecture pattern in a repository. Use when starting a new project or adding architectural discipline to an existing codebase.
---

# Architecture Initialization Skill

## Purpose
Bootstrap the progressive decomposition architecture pattern in a repository.

## When to Use
- Starting a new project
- Adding architectural discipline to an existing codebase
- User explicitly requests "initialize architecture" or similar

## Process

### 1. Assess Current State
- Check if root ARCHITECTURE.md exists
- Scan repository structure to identify major directories/components
- Look for existing documentation patterns

### 2. Create Root ARCHITECTURE.md
If no root architecture exists, create `/ARCHITECTURE.md` using the schema defined in ARCHITECTURE_SCHEMA.md:

```markdown
---
managed-by: arch-plugin
schema-version: 1.0.0
doc-version: 1.0.0
last-updated: [YYYY-MM-DD]
---

# [Project Name]

## Purpose
[High-level system purpose - what problem does this solve?]

## External Interfaces

### Provides (Ports)
- [What does the system expose to external users/systems?]

### Requires (Ports)
- [What external dependencies does the system need?]

## Internal Architecture

### Subcomponents
- **ComponentName** (at `./path/`): Brief purpose
[List major subsystems/modules]

### Internal Interfaces (Connections)
[How do top-level components interact?]

## Notes
Architecture initialized on [DATE]

---
*Parent: root*
```

### 3. Identify Candidate Components
Scan the repository for directories that likely represent architectural components:
- Directories with multiple source files
- Directories with clear domain boundaries (auth, api, data, etc.)
- Existing package/module structures

### 4. Propose Component Documentation
For each identified component, suggest creating an ARCHITECTURE.md:
- Present the list to the user
- Ask which components should get immediate documentation
- Offer to create skeleton ARCHITECTURE.md files for selected components

### 5. Create Component Documentation
For approved components, create `[component-path]/ARCHITECTURE.md`:
- Use the schema from ARCHITECTURE_SCHEMA.md
- Include required frontmatter with:
  - `managed-by: arch-plugin`
  - `schema-version: 1.0.0`
  - `doc-version: 1.0.0`
  - `last-updated: [current date]`
- Analyze the code in that directory
- Identify subcomponents (subdirectories or major classes)
- Infer external interfaces (what's exported/public)
- Infer internal interfaces (how pieces interact)
- Add parent reference back to root

### 6. Establish Consistency
- Ensure root ARCHITECTURE.md lists all documented components
- Verify parent references are correct
- Check that external interfaces align between levels

## Output
- Root ARCHITECTURE.md created/updated
- Component ARCHITECTURE.md files for selected components
- Summary report of what was created
- Suggestions for areas needing human review

## Notes
- Keep initial documentation lightweight - capture structure, not every detail
- Flag areas where purpose/interfaces are unclear for human input
- Prefer creating simple docs over no docs
- Update timestamps on all modified files
