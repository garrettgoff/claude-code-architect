---
description: Run the full architecture workflow with proposal, review, and implementation
---

# Architecture Workflow

You are orchestrating a multi-phase architecture workflow for a code change. This workflow ensures architectural consistency through progressive decomposition and adversarial design review.

## User Request

The user wants: "$ARGUMENTS"

## Workflow Phases

### Phase 1: Initialization Check
- Verify that root ARCHITECTURE.md exists
- If missing, ask if user wants to initialize architecture first
- Use architecture-init skill if needed

### Phase 2: Context Gathering
- Use architecture-query skill to understand current state
- Identify affected components
- Load current interfaces and dependencies
- Build a map of the architectural context

### Phase 3: Trivial Check
- Assess if the change is an internal implementation detail
- If it doesn't affect interfaces or boundaries, ask: "This appears to be an internal change to [Component]. Skip formal architecture review?"
- If user approves skip, go directly to Phase 7 (Implementation)

### Phase 4: Design Proposal
- Use architecture-proposer skill to create a design proposal
- Identify architectural impacts
- Document required ARCHITECTURE.md updates
- Present proposal to user with clear explanation

### Phase 5: User Approval Gate
- Show the proposal to the user
- Ask: "Review the architectural design proposal. Approve to proceed?"
- Options: Approve, Revise, Reject
- If revise: gather feedback and return to Phase 4
- If reject: stop workflow

### Phase 6: Adversarial Review
- Use architecture-reviewer skill to validate the proposal
- Check consistency (upward, downward, horizontal)
- Verify architectural principles
- Detect drift and anti-patterns
- Generate review report with verdict: APPROVED | APPROVED WITH CONCERNS | NEEDS REVISION | REJECTED

**Handle Review Verdict:**
- **REJECTED**: Stop and show review report
- **NEEDS REVISION**: Return to Phase 4 with required changes
- **APPROVED WITH CONCERNS**: Ask user to confirm: "Design approved with concerns. Review concerns and confirm to proceed?"
- **APPROVED**: Continue to Phase 7

### Phase 7: Update Architecture Docs
- Apply the documented ARCHITECTURE.md changes
- Update all affected files
- Maintain consistency between parent and child docs
- Verify timestamps are updated

**If creating new components:**
1. Create the component directory: `mkdir -p ./path/to/component`
2. Create `./path/to/component/ARCHITECTURE.md` with proper frontmatter:
   ```yaml
   ---
   managed-by: arch-plugin
   schema-version: 1.0.0
   doc-version: 1.0.0
   last-updated: [DATE]
   ---
   ```
3. Populate Purpose, External Interfaces, Internal Architecture sections
4. Add parent reference at bottom
5. Update parent's ARCHITECTURE.md to list the new subcomponent
6. Verify bidirectional references are correct

### Phase 8: Implementation
- Implement the code changes
- Stay within documented component boundaries
- Follow the interfaces specified in ARCHITECTURE.md
- Reference the design proposal during implementation

**For new components:**
- Create code files inside the component directory
- Ensure exports match the "Provides" section in ARCHITECTURE.md
- Ensure imports match the "Requires" section in ARCHITECTURE.md
- If adding subcomponents within the new component, create nested ARCHITECTURE.md files as needed

### Phase 9: Verification
- Use architecture-query skill to verify implementation
- Check that code matches architectural intent
- Flag any deviations
- Generate verification report

### Phase 10: Complete
- Present summary to user:
  - Files implemented
  - Architecture docs updated
  - Verification status
- Confirm workflow is complete

## Error Handling

- If any phase fails, report the error and stop
- If consistency violations are found, report to reviewer
- Allow user to abort at any approval gate

## Configuration

- By default, require explicit approval at gates
- Use verbose output to keep user informed
- Always run adversarial review for non-trivial changes

## Notes

- This workflow is designed for non-trivial changes that affect architecture
- For simple bug fixes or internal changes, use the trivial check to skip review
- The adversarial review is intentionally skeptical to prevent architectural decay
- Keep user informed at each phase
- **Implementation is architecture-guided** - the plugin ensures code follows approved design
- **Verification checks architectural consistency**, not functional correctness
- For comprehensive testing (unit, integration, E2E), use a separate test plugin or run tests manually
