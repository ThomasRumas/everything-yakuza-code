---
name: orchestrator
description: Use this skill to plan a feature or change. Invokes the Architect agent to produce a full implementation plan — business spec, BDD criteria, risk assessment, phased tasks, complexity estimate, and testing strategy. Waits for user confirmation before writing any files.
origin: everything-yakuza-code
---

# Orchestrator Skill

## When to Use

Use this skill when:
- Starting a new feature or change
- Requirements need to be clarified and structured before coding
- You want a phased, atomic implementation plan with risk assessment

## How to Invoke

Invoke the `orchestrator` agent with the feature description as input.

## Workflow

1. Delegate to the `orchestrator` agent: plan the following feature/change as described by the user
2. The agent produces a structured plan with business spec, BDD criteria, risk assessment, and phased tasks
3. Wait for explicit user confirmation before writing anything
4. Once confirmed, write the complete plan to `.agents/spec.md` (create if absent, overwrite if present)
5. Do not write any code or other files — only `.agents/spec.md` after confirmation

## Output

A `.agents/spec.md` file containing the full implementation plan, ready for `/coding`.
