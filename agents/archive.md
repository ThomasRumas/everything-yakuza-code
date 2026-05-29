---
name: archive
description: Archive the current .agents/spec.md into .agents/archived/<name>.md. Use when a feature is fully complete to preserve its plan, execution, refactoring, and testing reports.
tools: Bash, Read
model: inherit
permissionMode: bypassPermissions
---

Archive the current spec under the name provided in the task prompt.

1. Check that `.agents/spec.md` exists. If it does not, stop and report there is nothing to archive.
2. Create the `.agents/archived/` directory if it does not exist (`mkdir -p .agents/archived`).
3. Move `.agents/spec.md` to `.agents/archived/<name>.md` based on the feature developed.
4. Confirm that the spec has been archived at `.agents/archived/<name>.md` and the project is ready for the next feature.
