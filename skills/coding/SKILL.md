---
name: coding
description: Use this skill to execute an implementation plan. Reads .agents/spec.md and follows the plan exactly — creates and modifies files task by task, validates acceptance criteria, and appends an Execution Report to spec.md.
origin: everything-yakuza-code
---

# Coding Skill

## When to Use

Use this skill when:
- An implementation plan exists in `.agents/spec.md`
- You are ready to execute the plan produced by `/orchestrator`

## How to Invoke

Invoke the `coding` agent to execute the plan from `.agents/spec.md`.

## Workflow

1. Read `.agents/spec.md`. If the file does not exist, stop and tell the user to run `/orchestrator` first.
2. Delegate to the `coding` agent to execute every task in the `# Implementation Plan` section exactly as written.
3. The agent must not make decisions, interpret ambiguities, or add unspecified code.
4. Once all tasks are complete, append the following section to `.agents/spec.md`:

```
## Execution Report
<Execution Report output here>
```

## Output

An updated `.agents/spec.md` with an `## Execution Report` section appended, listing completed tasks, modified files, and acceptance criteria validation.
