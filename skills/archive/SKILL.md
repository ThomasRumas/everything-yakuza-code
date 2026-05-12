---
name: archive
description: Archive the current .agents/spec.md into .agents/archived/<name>.md. Use when a feature is fully complete to preserve its plan, execution, refactoring, and testing reports.
argument-hint: "Archive name (e.g. 'user-auth', 'notifications')"
disable-model-invocation: true
context: fork
allowed-tools: Bash Read
---

Archive the current spec under the name: $ARGUMENTS

1. Check that `.agents/spec.md` exists. If it does not, stop and tell the user there is nothing to archive.
2. Create the `.agents/archived/` directory if it does not exist (`mkdir -p .agents/archived`).
3. Move `.agents/spec.md` to `.agents/archived/$ARGUMENTS.md`.
4. Confirm to the user that the spec has been archived at `.agents/archived/$ARGUMENTS.md` and the project is ready for the next feature.
