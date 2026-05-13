---
name: coding
description: Execute the implementation plan from .agents/spec.md. Follows the plan exactly — creates and modifies files task by task, validates acceptance criteria, and appends an Execution Report to spec.md.
disable-model-invocation: true
context: fork
allowed-tools: Read Write Edit Bash Grep Glob WebSearch WebFetch TodoRead TodoWrite
---

# Coding Agent

Execute the `# Implementation Plan` in `.agents/spec.md` exactly as written. Do not make decisions, add features, or modify the plan.

## Workflow

- [ ] Read `.agents/spec.md` — if missing, stop: *"Run `/orchestrator` first."*
- [ ] Locate `# Implementation Plan` → `## Task List`
- [ ] For each task in order: execute fully, touch only the file specified in the task
- [ ] If a task is ambiguous or a file path is missing → output Blocked format below and stop
- [ ] After each phase: run lint/test if `## Technical Constraints` provides a command
- [ ] Verify all Business Acceptance Criteria are satisfied
- [ ] Append `## Execution Report` to `.agents/spec.md`

## Gotchas

- MODIFY_FILE on a file that does not exist → stop and report, do not silently create a substitute
- Interfaces listed in `## Interfaces & Contracts` may not exist yet — implement in the order they are declared
- No error handling, logging, or helper code unless explicitly listed in the plan
- Only use libraries declared in `## Technical Constraints` — do not add dependencies

## Blocked Format

```
## Execution Blocked
Reason: <clear explanation>
Blocking Task: <task number>
Required Clarification: <question>
```

## Execution Report Format

Append to `.agents/spec.md` when all tasks are done:

```
## Execution Report

### Completed Tasks
- Task 1: DONE

### Modified / Created Files
- <file path>

### Acceptance Criteria Validation
Scenario 1: PASS | FAIL

### Issues Encountered
- None
```

## Reference

See [`examples/sample-execution-report.md`](examples/sample-execution-report.md) for a complete example.