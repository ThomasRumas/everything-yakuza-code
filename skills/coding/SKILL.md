---
name: coding
description: Execute the implementation plan from .agents/spec.md. Follows the plan exactly — creates and modifies files task by task, validates acceptance criteria, and appends an Execution Report to spec.md.
disable-model-invocation: true
allowed-tools: Read Write Edit Bash Grep Glob WebSearch WebFetch TodoRead TodoWrite
---

# Coding Agent

Execute the `# Implementation Plan` in `.agents/spec.md` exactly as written. Do not make decisions, add features, or modify the plan.

## Critical Rules

- **NEVER write unit tests or integration tests** — testing is handled exclusively by `/testing`
- **Frontend verification**: when implementing frontend code (HTML, CSS, React, Vue, etc.), use `playwright-cli` to verify the UI renders correctly after each significant change

## Workflow

- [ ] Read `.agents/spec.md` — if missing, stop: *"Run `/orchestrator` first."*
- [ ] Locate `# Implementation Plan` → `## Task List`
- [ ] For each task in order: execute fully, touch only the file specified in the task
- [ ] If a task is ambiguous or a file path is missing → output Blocked format below and stop
- [ ] After each phase: run lint/test if `## Technical Constraints` provides a command
- [ ] **Frontend tasks**: after modifying UI files, verify with `playwright-cli`:
  ```
  playwright-cli open
  playwright-cli goto <local-url>
  playwright-cli screenshot
  ```
  Fix any visual or functional issues before moving to the next task
- [ ] Verify all Business Acceptance Criteria are satisfied
- [ ] Append `## Execution Report` to `.agents/spec.md`

## Frontend Verification

When working on frontend code, use `playwright-cli` to visually confirm the implementation:

1. **Open a browser**: `playwright-cli open`
2. **Navigate**: `playwright-cli goto http://localhost:<port>`
3. **Inspect the page snapshot**: `playwright-cli snapshot` (preferred over screenshot — shows DOM structure with ref IDs)
4. **Interact if needed**: `playwright-cli click <ref>`, `playwright-cli type "text"`, `playwright-cli press Enter`
5. **Verify state**: check the snapshot output matches expected UI behavior
6. **Close when done**: `playwright-cli close`

If `playwright-cli` is not available, fall back to running the app and checking console output for errors.

## Gotchas

- **DO NOT write tests** — no unit tests, no integration tests, no test files whatsoever. Testing is the sole responsibility of `/testing`
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