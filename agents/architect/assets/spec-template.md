# Spec Template

Use this exact structure for `.agents/spec.md`. Every section is mandatory.
Non-applicable sections must include `N/A — Reason: <explanation>`.

---

# Business Specification

## Business Context
- Problem:
- Goal:
- Users:
- Expected Outcome:

## Business Acceptance Criteria (BDD)

Scenario 1:
Given ...
When ...
Then ...

Scenario 2:
...

## Edge Cases
- Case 1:
- Case 2:

## Risk Assessment

| Severity | Risk | Mitigation |
|----------|------|------------|
| HIGH | <risk description> | <mitigation> |
| MEDIUM | <risk description> | <mitigation> |
| LOW | <risk description> | <mitigation> |

---

# Technical Specification

## Architecture Overview
- Components:
- Data Flow:
- External Dependencies:

## Technical Constraints
- Language:
- Framework:
- Runtime:
- Performance:
- Security:

## Design Decisions

Decision 1:
- Choice:
- Reason:
- Alternatives Considered:

---

# Implementation Plan

## Task List (Atomic & Ordered)

Tasks are grouped into named phases. Each task modifies exactly one file.

### Phase 1: <Phase Name>

Task 1:
- Type: CREATE_FILE | MODIFY_FILE | DELETE_FILE
- Path: <exact file path>
- Details:
  - <precise instruction>

---

## Complexity Estimation

| Area | Estimate |
|------|----------|
| **Total** | **<X–Y hours>** |

Complexity: HIGH | MEDIUM | LOW

---

## File Dependency Graph

fileA -> fileB

---

## Interfaces & Contracts

Interface: <Name>
Methods:
- methodName(params): ReturnType

---

# Testing Strategy

## Unit Tests
- File: <path>
- Cases:
  - ...

## Integration Tests
- Flow:
  - ...

## Mocking Strategy
- ...

---

# Definition of Done

- [ ] Code compiles
- [ ] Lint passes
- [ ] Tests pass
- [ ] No TODO comments
- [ ] All acceptance criteria satisfied

---

# Assumptions

1. ...

---

## Validation Checklist

Before writing to `.agents/spec.md`, verify every item:

- [ ] All sections above are present (none skipped)
- [ ] Every task has an exact file path
- [ ] No task contains multiple responsibilities
- [ ] No implicit decisions — every choice is stated
- [ ] Non-applicable sections include a reason
- [ ] Plan ends with `WAITING FOR CONFIRMATION: Reply **yes** to proceed, or describe any changes.`
