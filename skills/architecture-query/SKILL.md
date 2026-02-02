---
name: architecture-query
description: Understand and navigate the current architectural state of a codebase. Use when you need to understand component structure, interfaces, dependencies, or architecture before making changes.
---

# Architecture Query Skill

## Purpose
Help users and other skills understand and navigate the current architectural state of a codebase.

## When to Use
- User asks "what's the architecture of..." or "how does X work?"
- Other skills need to understand component structure before making changes
- Before starting a new task to understand context
- When re-entering a project after time away

## Query Types

### 1. Component Overview
**Query:** "What is [component] responsible for?"

**Process:**
- Locate the component's ARCHITECTURE.md
- Read its Purpose section
- Summarize its role in the system
- Show its location in the hierarchy

**Output:**
```
[ComponentName] is responsible for [purpose].

Location: /path/to/component/
Parent: [ParentComponent]

Key responsibilities:
- [Responsibility 1]
- [Responsibility 2]
```

### 2. Interface Discovery
**Query:** "What interfaces does [component] provide?" or "What does [component] depend on?"

**Process:**
- Read component's ARCHITECTURE.md
- Extract External Interfaces section
- Distinguish between provides and requires
- Show interface signatures

**Output:**
```
[ComponentName] External Interfaces:

Provides (Ports):
- function_name(params) -> return_type: Description
- ...

Requires (Ports):
- DependencyComponent: function_name(params) -> return_type
- ...
```

### 3. Dependency Tracing
**Query:** "What depends on [component]?" or "Show me the dependency graph for [component]"

**Process:**
- Find all ARCHITECTURE.md files in the repo
- Search for references to the component in "Requires" sections
- Build upstream dependency list
- Show direct and transitive dependencies

**Output:**
```
Components depending on [ComponentName]:

Direct dependencies:
- ComponentA uses: function_x(), function_y()
- ComponentB uses: function_z()

Transitive dependencies:
- ComponentC → ComponentA → [ComponentName]
```

### 4. Component Hierarchy
**Query:** "Show me the structure of [component]" or "What subcomponents does [component] have?"

**Process:**
- Read component's ARCHITECTURE.md
- Extract Subcomponents list
- Recursively read subcomponent ARCHITECTURE.md files
- Build hierarchical tree

**Output:**
```
[ComponentName] hierarchy:
├── SubcomponentA (./subA/)
│   ├── SubSubA1 (./subA/sub1/)
│   └── SubSubA2 (./subA/sub2/)
└── SubcomponentB (./subB/)
    └── Single-file implementation
```

### 5. Architecture Search
**Query:** "Where should I put functionality that does [X]?" or "Which component handles [Y]?"

**Process:**
- Read all ARCHITECTURE.md Purpose sections
- Match against query semantically
- Rank components by relevance
- Show top matches with reasoning

**Output:**
```
Best match for "[functionality]":

1. [ComponentName] (confidence: high)
   Reason: Purpose explicitly mentions [related concept]
   Location: /path/

2. [ComponentName2] (confidence: medium)
   Reason: Related but slightly broader scope
   Location: /path/

Recommendation: [Which component and why, or suggest creating new component]
```

### 6. Consistency Check
**Query:** "Check architecture consistency" or "Audit architecture"

**Process:**
- Load all ARCHITECTURE.md files
- Verify parent-child references
- Check interface consistency (provides/requires alignment)
- Look for broken links or missing docs
- Report findings

**Output:**
```
Architecture Consistency Report:

✓ All parent references valid
✓ No circular dependencies detected
✗ 2 interface mismatches found:
  - ComponentA requires ComponentB.func_x() but ComponentB doesn't provide it
  - ComponentC's parent doc lists SubD but SubD has no ARCHITECTURE.md

Warnings:
- 5 directories with multiple source files but no ARCHITECTURE.md

Recommendations:
[Specific fixes needed]
```

### 7. Impact Analysis
**Query:** "What would be affected if I change [component/interface]?"

**Process:**
- Identify the component or interface
- Find all consumers (via dependency tracing)
- Find all subcomponents (via hierarchy)
- Assess blast radius

**Output:**
```
Impact analysis for changing [ComponentName.function()]:

Direct impact:
- 3 components call this function directly
- 2 internal subcomponents implement it

Indirect impact:
- 5 components depend on the direct consumers
- Total affected components: 10

Risk level: [HIGH/MEDIUM/LOW]

Recommendation: [Whether change is safe, needs migration plan, etc.]
```

## Helper Functions

### Load Component Context
Given a component name or path, load its ARCHITECTURE.md and return structured data:
```python
{
  "name": "ComponentName",
  "path": "/path/to/component",
  "purpose": "...",
  "external_provides": [...],
  "external_requires": [...],
  "subcomponents": [...],
  "internal_connections": [...],
  "parent": "...",
  "last_updated": "..."
}
```

### Find Component by Name
Search all ARCHITECTURE.md files to find a component by name (handles ambiguity if multiple matches).

### Build Full Dependency Graph
Parse all architecture docs and build complete provides/requires graph for the entire system.

### Validate References
Check that all mentioned components, subcomponents, and interfaces actually exist in the documented architecture.

## Output Formats

### Concise (default)
Brief answer to specific query, minimal context.

### Detailed
Full information including rationale, alternatives, related components.

### Machine-Readable
Structured JSON for consumption by other skills:
```json
{
  "component": "...",
  "provides": [...],
  "requires": [...],
  "subcomponents": [...],
  "relationships": [...]
}
```

## Usage by Other Skills

Other skills can invoke architecture-query to:
- **architecture-proposer**: Understand current state before proposing changes
- **architecture-reviewer**: Load context for consistency checking
- **implementation skills**: Determine where to place new code
- **test skills**: Identify interfaces that need testing

## Special Cases

### No Architecture Documentation
If ARCHITECTURE.md files don't exist:
- Suggest running architecture-init first
- Offer to infer structure from code (with caveats)
- Can still scan code to give basic answers

### Ambiguous Queries
If query matches multiple components or interpretations:
- Present all matches
- Ask user to clarify
- Offer to show comparison

### Stale Documentation
If code appears to have diverged from docs:
- Flag discrepancy
- Show both documented and actual state
- Recommend architecture audit

## Notes
- This skill is read-only - it never modifies ARCHITECTURE.md files
- Designed to be fast and lightweight for frequent consultation
- Can be invoked by users or programmatically by other skills
- Should cache loaded architecture data within a session to avoid repeated file reads
