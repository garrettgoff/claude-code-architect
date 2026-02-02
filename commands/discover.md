---
description: Reverse-engineer architecture documentation from existing codebase by analyzing code structure
---

# Architecture Discovery

You are helping the user discover and document the architecture of an existing codebase by analyzing the code structure, dependencies, and organization.

## Process

### 1. Initial Scan
Use the architecture-discover skill to:
- Analyze the codebase structure
- Identify programming languages
- Map directory hierarchy
- Build dependency graph
- Identify natural component boundaries

### 2. Confidence Assessment
For each discovered component, assess confidence level:
- **HIGH**: Clear purpose, good docs, clean boundaries
- **MEDIUM**: Reasonable structure but some ambiguity
- **LOW**: Unclear purpose, poor organization, needs review

### 3. Present Findings
Show the user:
- Discovered component hierarchy
- Confidence levels for each component
- Detected architectural issues (circular deps, god modules, etc.)
- Recommendations for improvements

Ask: "Should I proceed with generating ARCHITECTURE.md files for these components?"

### 4. Generate Documentation
For approved components:
- Create ARCHITECTURE.md files using the schema
- Include frontmatter with `managed-by: arch-plugin`
- Set `doc-version: 1.0.0` (initial version)
- Flag uncertain sections with notes
- Mark stability based on analysis

### 5. Generate Discovery Report
Create a summary report showing:
- Total components discovered
- Confidence breakdown
- Detected issues
- Recommendations
- Areas needing human review

### 6. Interactive Refinement
Present the report and ask:
- Which low-confidence components need clarification?
- Should any components be split or merged?
- Are the inferred purposes accurate?

Iterate based on feedback.

### 7. Validation
Run consistency checks:
- Verify parent-child references
- Check interface alignment
- Validate no broken links
- Ensure schema compliance

## User Input

The user may provide:
- Specific directories to analyze: `$ARGUMENTS`
- Or analyze entire repository if no arguments

If arguments provided, focus discovery on those paths. Otherwise, start from repository root.

## Important Notes

- Generated docs are a **starting point**, not final truth
- Always flag uncertainties for human review
- Prefer generating simple docs over nothing
- Mark sections that need validation
- Use code comments, READMEs, and git history for context
- When in doubt, ask the user for clarification

## Example Usage

```bash
/arch:discover                    # Discover entire repo
/arch:discover src/               # Discover src/ directory only
/arch:discover backend/ frontend/ # Discover multiple directories
```

## Output

At completion, provide:
1. Summary of components discovered
2. Confidence levels
3. List of generated ARCHITECTURE.md files
4. Detected issues and recommendations
5. Next steps for refinement
