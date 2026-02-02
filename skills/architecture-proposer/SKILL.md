---
name: architecture-proposer
description: Analyze requested changes and propose architectural updates, including impacts to ARCHITECTURE.md files. Use before implementing non-trivial features or refactoring.
---

# Architecture Proposer Skill

## Purpose
Analyze a requested change and propose architectural updates, including impacts to ARCHITECTURE.md files.

## When to Use
- Before implementing any non-trivial feature
- Before refactoring that might affect component boundaries
- When user asks for design analysis
- When uncertain about where new functionality belongs

## Process

### 1. Understand the Request
- Parse the user's change request
- Identify the core functionality being added/modified
- Determine scope (new feature, bug fix, refactor, etc.)

### 2. Locate Affected Components
- Read root ARCHITECTURE.md
- Identify which existing components are involved
- Follow parent/child links to load relevant component ARCHITECTURE.md files
- Build a map of affected components and their relationships

### 3. Analyze Architectural Impact

**For each affected component, determine:**

a) **Interface Changes**
   - Are new external ports being added?
   - Are existing ports being modified?
   - Are new dependencies (required ports) being introduced?
   - Do internal connections change?

b) **Component Boundary Questions**
   - Does this functionality fit within an existing component's purpose?
   - Does it require a new component?
   - Does it blur boundaries between components?
   - Should existing components be split or merged?

c) **Complexity Assessment**
   - Is a component growing beyond single-file implementation?
   - Should a class cluster be promoted to subcomponent status?
   - Are there >3-4 collaborating classes without explicit architecture?

### 4. Generate Proposal Document

Create a structured proposal:

```markdown
# Design Proposal: [Change Description]

## Requested Change
[What the user wants to accomplish]

## Affected Components
- Component A (at ./path/): [How it's affected]
- Component B (at ./path/): [How it's affected]

## Proposed Architectural Changes

### New/Modified External Interfaces
**Component A:**
- Add port: `new_function(params)` -> return_type
- Modify port: `existing_function()` - [explain change]

**Component A requires:**
- New dependency on Component C: `some_function()`

### New/Modified Internal Architecture
**Component A:**
- Add subcomponent: NewSubcomponent (purpose: ...)
- New internal connection: SubA → SubB via interface X

### New Components (if applicable)
**ComponentName** (proposed location: ./path/)
- Purpose: ...
- External Ports: ...
- Internal Architecture: ...

## ARCHITECTURE.md Updates Required
- `/ARCHITECTURE.md`: Add ComponentName to subcomponents
- `/path/to/ComponentA/ARCHITECTURE.md`: Update external interfaces, add subcomponent
- `/path/to/ComponentB/ARCHITECTURE.md`: Add dependency on ComponentA.new_function

## Open Questions
- [Any ambiguities or design alternatives]
- [Areas needing human decision]

## Implementation Notes
[High-level implementation approach, key classes/files to modify]
```

### 5. Present Proposal
- Show the proposal to the user
- Highlight key architectural decisions
- Ask for feedback before proceeding
- Flag any violations of existing patterns

### 6. Handle Feedback
- If approved: proceed to update ARCHITECTURE.md files as specified
- If changes requested: revise proposal
- If rejected: explain why and suggest alternatives

## Special Cases

### Trivial Changes
If the change is clearly localized and doesn't affect architecture:
- State: "This appears to be an internal implementation detail of [Component]"
- Confirm it doesn't add interfaces or dependencies
- Skip formal proposal if user agrees

### Unclear Component Ownership
If unsure where functionality belongs:
- Present 2-3 alternatives with tradeoffs
- Explain architectural implications of each
- Let user decide or provide reasoning for recommendation

### Promotion from Docstring to Architecture
If encountering a class cluster that should be promoted:
- Flag: "This appears to be a logical component without formal documentation"
- Propose creating ARCHITECTURE.md for it
- Migrate relevant docstring content as starting point

## Output
- Design proposal document
- List of ARCHITECTURE.md files to update
- Clear recommendation: approve/revise/reject
- Updated architecture files (after approval)

## Quality Checks
- Every external port change is documented
- Every new dependency is explicit
- Parent/child consistency is maintained
- Purpose statements remain clear and focused

## Version Management

When proposing updates to ARCHITECTURE.md files, determine the appropriate doc-version bump:

**MAJOR (X.0.0)** - Breaking changes:
- Component purpose fundamentally changes
- External interface signatures change (breaking)
- External interfaces removed
- Component split or merged

**MINOR (0.X.0)** - Backwards-compatible additions:
- New external interfaces added
- New subcomponents added
- New dependencies added
- Significant internal architecture expansion

**PATCH (0.0.X)** - Documentation updates:
- Clarifications to existing docs
- Notes or examples added
- Internal implementation details updated
- No structural or interface changes

Include version bump reasoning in proposals:
```
ARCHITECTURE.md Updates:
- ./auth/ARCHITECTURE.md: 1.2.0 → 2.0.0 (MAJOR: changed login() signature)
- ./ARCHITECTURE.md: 1.0.0 → 1.1.0 (MINOR: added new OAuth component)
```
