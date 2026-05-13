---
name: testing
description: Generate unit and integration tests from .agents/spec.md. Maps every acceptance criterion to at least one test case, covers edge cases and failure modes, and appends a Testing Report to spec.md. Never modifies production code.
disable-model-invocation: true
context: fork
allowed-tools: Read Write Edit Bash Grep Glob WebSearch WebFetch TodoRead TodoWrite
---

# Testing Agent

Generate tests from `.agents/spec.md`. You must NOT modify production code — only produce test files.

## Workflow

- [ ] Read `.agents/spec.md` — if missing, stop: *"Run `/orchestrator` first."*
- [ ] Confirm `## Execution Report` exists — if missing, stop: *"Run `/coding` first."*
- [ ] If no test framework is specified in `## Technical Constraints` → use `AskUserQuestion` to ask before generating any files
- [ ] Map each BDD scenario in `## Business Acceptance Criteria` to ≥1 test case
- [ ] Map each item in `## Edge Cases` to ≥1 test case
- [ ] Identify failure modes implied by the implementation but not listed in the spec
- [ ] Self-validate: no production file was modified or created
- [ ] Append `## Testing Report` to `.agents/spec.md`

## Gotchas

- Test file paths must mirror source paths: `src/auth/login.ts` → `tests/auth/login.test.ts`
- Never read from or write to production files during test generation
- Derive all tests from the spec and implemented behavior only — do not invent new requirements
- Each acceptance criterion must map to ≥1 test — a missing mapping is a failure

## Testing Report Format

Append to `.agents/spec.md` when complete:

```
## Testing Report

### Coverage Summary
- Business Scenarios Covered: YES | NO
- Edge Cases Covered: YES | NO
- Overall Coverage: <estimate>

### Missing Coverage Analysis
- Missing Test 1: Related Feature / Missing Scenario / Risk if untested

### Test Plan

#### Unit Tests
Test File: <path>
Cases:
- Test 1: Given / When / Then

#### Integration Tests
Test File: <path>
Flow: Step 1 → Step 2 — Expected: <result>

### Mocking Strategy
- External services mocked: <list>

### Acceptance Criteria Validation
Scenario 1: PASS | FAIL — Covered by test: YES | NO
```

## Reference

See [`examples/sample-testing-report.md`](examples/sample-testing-report.md) for a complete example.