---
name: archive
description: Use this skill when a feature is fully complete (plan, execution, refactoring, and tests all done). Archives .agents/spec.md into .agents/archived/<name>.md to preserve the full development history.
origin: everything-yakuza-code
argument-hint: "Archive name (e.g. 'user-auth', 'notifications')"
---

# Archive Skill

## When to Use

Use this skill when:
- All pipeline stages are complete: plan, execution, refactoring (if applicable), and testing
- You want to preserve the spec before starting the next feature

## How to Invoke

Invoke with an archive name as argument (e.g. `user-auth`, `notifications`).

## Workflow

1. Check that `.agents/spec.md` exists. If it does not, stop and tell the user there is nothing to archive.
2. Create the `.agents/archived/` directory if it does not exist.
3. Move `.agents/spec.md` to `.agents/archived/<name>.md` where `<name>` is the argument provided by the user.
4. Confirm to the user that the spec has been archived at `.agents/archived/<name>.md`.

## Output

`.agents/spec.md` is moved to `.agents/archived/<name>.md`. The project is ready for the next feature.
