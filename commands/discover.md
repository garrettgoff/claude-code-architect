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

**CRITICAL: Create a MODULAR file structure with MULTIPLE ARCHITECTURE.md files.**

Create one ARCHITECTURE.md file in EACH component directory:
```
./ARCHITECTURE.md                    # Root - always create
./src/auth/ARCHITECTURE.md           # Each component gets its own file
./src/auth/session/ARCHITECTURE.md   # Subcomponents too (if warranted)
./src/api/ARCHITECTURE.md
./src/database/ARCHITECTURE.md
```

**Do NOT create a single monolithic file. The progressive decomposition pattern requires separate files per component.**

Each file must have this exact frontmatter:
```yaml
---
managed-by: arch-plugin
schema-version: 1.0.0
doc-version: 1.0.0
last-updated: [TODAY'S DATE]
---
```

Also:
- Flag uncertain sections with notes
- Optionally add `stability: stable|experimental` based on analysis
- Ensure parent references point to the correct parent ARCHITECTURE.md

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
3. **List of ALL generated ARCHITECTURE.md files with their paths:**
   ```
   Created ARCHITECTURE.md files:
     ✓ ./ARCHITECTURE.md (root)
     ✓ ./src/auth/ARCHITECTURE.md
     ✓ ./src/api/ARCHITECTURE.md
     ✓ ./src/database/ARCHITECTURE.md
   Total: 4 architecture files created
   ```
4. Detected issues and recommendations
5. Next steps for refinement

**Verification**: If only one ARCHITECTURE.md was created, the discovery was incomplete. Go back and create files for each component directory.
