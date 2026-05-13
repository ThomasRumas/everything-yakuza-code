---
name: refactor
description: Review code produced by /coding and propose safe improvements (critical, major, minor). No code is modified — all changes require explicit user approval. Appends a Refactoring Report to spec.md.
disable-model-invocation: true
context: fork
allowed-tools: Read Bash Grep Glob WebSearch WebFetch TodoRead TodoWrite
---

# Refactoring Agent

Review code produced by `/coding` and propose safe improvements. You must NOT modify any file — all proposals require explicit user approval.

## Workflow

- [ ] Read `.agents/spec.md` — if missing, stop: *"Run `/orchestrator` first."*
- [ ] Confirm `## Execution Report` exists — if missing, stop: *"Run `/coding` first."*
- [ ] Extract the file list from `## Modified / Created Files` inside `## Execution Report` — review only those files
- [ ] For each file: classify each finding as Critical / Major / Minor
- [ ] Apply the Safe Refactoring Rule to every finding — reject any that fail
- [ ] Self-validate: no HIGH-risk items in proposals; no files outside Execution Report scope
- [ ] Append `## Refactoring Report` to `.agents/spec.md`, then present to the user and wait for explicit approval

## Safe Refactoring Rule

Every proposed change must satisfy all conditions:

- **Behavior preserved** — same inputs → same outputs, no API contract change
- **Atomic** — independently applicable, one concern, reversible
- **Scope isolated** — limited to files listed in the Execution Report
- **Testable** — includes a validation step
- **Dependency safe** — no new dependencies, no library upgrades
- **Risk level** — LOW or MEDIUM only; HIGH → move to Rejected Suggestions

## Gotchas

- HIGH risk = REJECT, always — even when the fix looks obviously safe
- `## Modified / Created Files` is a subsection of `## Execution Report` — parse from there, not from the plan's task list
- Do not review files present in the Implementation Plan but absent from the Execution Report
- Approval must be the word "apply" — casual confirmation ("looks good", "sure") does not trigger changes

## Refactoring Report Format

Append to `.agents/spec.md` when complete:

```
## Refactoring Report

### Summary
- Critical: <n> | Major: <n> | Minor: <n>

### Detailed Findings

#### Issue 1
- Category: Critical | Major | Minor
- File: <path> — Location: <function/line>
- Problem: <explanation>
- Proposed Change: <modification>
- Rationale: <why this improves the code>
- Safety Check: Behavior Preserved: YES | Risk Level: LOW | MEDIUM
- Validation: <how to verify>

### Rejected Suggestions
- Suggestion 1: Reason: <why> — Risk: HIGH

### Approval Required
No changes applied. Reply "apply" to proceed.
```

## Reference

See [`examples/sample-refactoring-report.md`](examples/sample-refactoring-report.md) for a complete example.