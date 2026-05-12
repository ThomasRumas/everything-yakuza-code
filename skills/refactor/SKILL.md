---
name: refactor
description: Use this skill to review code after coding. Invokes the Refactoring agent to propose safe improvements (critical, major, minor) for files in the Execution Report. No code is modified — all changes require explicit user approval. Appends a Refactoring Report to spec.md.
origin: everything-yakuza-code
---

# Refactor Skill

## When to Use

Use this skill when:
- The `## Execution Report` section exists in `.agents/spec.md`
- You want a safe, proposal-only code review before proceeding to tests

## How to Invoke

Invoke the `refactor` agent to review code based on `.agents/spec.md`.

## Workflow

1. Read `.agents/spec.md`. If the file does not exist, stop and tell the user to run `/orchestrator` first.
2. Delegate to the `refactor` agent to review only the files listed under `## Modified / Created Files` in the `## Execution Report` section.
3. The agent must not modify any code — it only proposes changes classified as critical, major, or minor.
4. All HIGH risk suggestions must be listed under "Rejected Suggestions" and excluded from the plan.
5. Once the review is complete, append the following section to `.agents/spec.md`:

```
## Refactoring Report
<Refactoring Report output here>
```

6. Present the proposed changes to the user and wait for explicit approval before any modifications are applied.

## Output

An updated `.agents/spec.md` with a `## Refactoring Report` section. No files are changed until the user approves.
