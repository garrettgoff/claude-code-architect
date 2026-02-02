---
name: architecture-reviewer
description: Validate proposed architectural changes against existing architecture, enforce consistency, and identify violations. Use after architecture-proposer to adversarially review proposals.
---

# Architecture Reviewer Skill

## Purpose
Validate proposed architectural changes against existing architecture, enforce consistency, and identify violations or drift.

## When to Use
- After architecture-proposer generates a proposal
- Before implementing changes that affect architecture
- When auditing existing code against documented architecture
- User requests architectural review

## Review Process

### 1. Load Architectural Context
- Read all ARCHITECTURE.md files affected by the proposal
- Build parent-child relationship graph
- Map all documented interfaces (external and internal)
- Identify current component boundaries

### 2. Consistency Checks

#### Upward Consistency
For each component being modified:
- Does parent's understanding of this component match its ARCHITECTURE.md purpose?
- Does parent's expectation of provided ports match component's documented external ports?
- Does parent's assumption about required ports match component's documented dependencies?

#### Downward Consistency
For each component with subcomponents:
- Do internal connections reference only ports that subcomponents actually provide?
- Are external ports implemented by documented subcomponents?
- Do subcomponent purposes align with parent's architectural intent?

#### Horizontal Consistency
For cross-component dependencies:
- If Component A requires port X from Component B, does B document providing X?
- Are interface signatures consistent (params, return types)?
- Are dependency directions acyclic (no circular dependencies)?

### 3. Architectural Principle Checks

#### Single Responsibility
- Does each component have a clear, focused purpose?
- Are we adding functionality that blurs a component's responsibility?
- Should a component be split?

#### Interface Segregation
- Are we exposing more than needed?
- Can we break large interfaces into smaller, focused ones?
- Are components depending on interfaces they don't fully use?

#### Dependency Management
- Are new dependencies justified?
- Do dependencies flow in the right direction (toward stable components)?
- Are we creating coupling that will be hard to change?

#### Encapsulation
- Does the change expose internal details?
- Are internal connections becoming external interfaces?
- Is implementation leaking into interfaces?

### 4. Drift Detection

Look for signs that code has diverged from architecture:
- Classes/files not mentioned in any ARCHITECTURE.md
- Dependencies used in code but not documented
- Interfaces used differently than documented
- Components that have outgrown their documented scope

### 5. Anti-Pattern Detection

Flag concerning patterns:
- God objects (components doing too much)
- Tight coupling (many cross-component connections)
- Circular dependencies
- Hidden dependencies (undocumented couplings)
- Shotgun surgery indicators (change affects many components)

### 6. Generate Review Report

```markdown
# Architecture Review: [Change Description]

## Overall Assessment
[APPROVED | APPROVED WITH CONCERNS | NEEDS REVISION | REJECTED]

## Consistency Findings

### ✓ Passed Checks
- [List consistency checks that passed]

### ⚠ Warnings
- [Non-blocking concerns that should be addressed]
- Location: [specific file/component]
- Recommendation: [how to address]

### ✗ Violations
- [Blocking issues that must be fixed]
- Location: [specific file/component]
- Impact: [why this matters]
- Required fix: [specific action needed]

## Architectural Drift
[Any divergence between code and documented architecture]

## Recommendations

### Must Address (Blocking)
1. [Critical issues]

### Should Address (Important)
1. [Significant concerns]

### Consider (Nice to have)
1. [Optional improvements]

## Approval Conditions
[If approved with concerns, what needs to happen before implementation?]

---
Reviewed: [TIMESTAMP]
```

### 7. Adversarial Stance

The reviewer should actively **look for problems**:
- Don't assume the proposal is correct
- Challenge interface decisions
- Question new dependencies
- Verify that simpler alternatives were considered
- Push back on complexity

This is constructive adversarialism - find issues before they become technical debt.

## Special Cases

### Legacy Code Without Architecture
If reviewing code that lacks architectural documentation:
- Flag: "No ARCHITECTURE.md found for [component]"
- Recommend running architecture-init first
- Can proceed with review but note increased risk

### Proposal Modifies Multiple Independent Components
If a change touches many unrelated components:
- Flag: "This change has broad architectural impact"
- Question whether it should be broken into smaller changes
- Verify it's not a sign of poor component boundaries

### Breaking Changes to External Interfaces
If a proposal modifies existing external ports:
- Flag: "BREAKING CHANGE detected"
- Require explicit justification
- Demand migration strategy for consumers
- Consider versioning strategy

## Review Metrics

Track and report:
- Number of components affected
- Number of interface changes (additions vs modifications)
- New dependencies introduced
- Architectural debt indicators (complexity, coupling)

## Output
- Architecture review report
- Clear verdict: APPROVED | APPROVED WITH CONCERNS | NEEDS REVISION | REJECTED
- Specific action items if not approved
- Risk assessment

## Quality Standards
- Every interface change is scrutinized
- Every new dependency is justified
- Consistency is verified at all levels
- Architectural principles are enforced
- Drift is identified and flagged
