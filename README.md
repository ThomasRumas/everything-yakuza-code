# Everything Yakuza Code

Agent-based software development workflow plugin for Claude Code.

A structured pipeline of specialized agents — plan, code, refactor, test — with a guided hook that tells you what to do next after every step.

---

## Install

```
# Add marketplace
/plugin marketplace add https://github.com/thomasrumas/everything-yakuza-code

# Install plugin
/plugin install eyc@eyc
```

---

## The Pipeline

```
/orchestrator "feature description"
       ↓
   [confirm plan]
       ↓
  /coding
       ↓
  /refactor
       ↓
   [approve changes]
       ↓
  /testing
       ↓
  /archive <name>
```

After every Claude response, a hook reads `.agents/spec.md` and prints the suggested next step — so you never have to remember where you are in the pipeline.

---

## Commands

| Command | What it does |
|---|---|
| `/orchestrator "feature"` | Architect agent produces a full implementation plan. Waits for your confirmation before writing anything. |
| `/coding` | Coding agent executes the plan from `.agents/spec.md` exactly as written. No interpretation, no extra work. |
| `/refactor` | Refactoring agent reviews the code and proposes safe improvements (critical / major / minor). No code is modified until you approve. |
| `/testing` | Testing agent generates unit and integration tests mapped to every acceptance criterion. Never touches production code. |
| `/archive <name>` | Moves `.agents/spec.md` to `.agents/archived/<name>.md` when the feature is complete. |

---

## Agents

Each command delegates to a specialized agent with a strict role:

- **orchestrator** — Transforms a user request into a complete, unambiguous implementation plan. Blocks if requirements are unclear.
- **coding** — Executes the plan task by task. Stops immediately if anything is ambiguous or missing.
- **refactor** — Proposes safe, behavior-preserving improvements only. Rejects anything HIGH risk.
- **testing** — Generates tests from acceptance criteria. Read-only access to production code.

---

## How `.agents/spec.md` works

All agents share a single file: `.agents/spec.md`.

Each pipeline stage appends its report to that file:

```
spec.md
├── # Business Specification       ← orchestrator writes
├── # Technical Specification      ← orchestrator writes
├── # Implementation Plan          ← orchestrator writes
├── ## Execution Report            ← coding appends
├── ## Refactoring Report          ← refactor appends
└── ## Testing Report              ← testing appends
```

The Stop hook reads which sections exist and guides you to the next step.

---

## Pipeline hook

After every Claude response, the hook checks `.agents/spec.md` state:

| State | Suggestion |
|---|---|
| No spec.md | *(silent)* |
| No Execution Report | `→ Next: run /coding` |
| No Refactoring Report | `→ Next: run /refactor` |
| Refactoring pending approval | `→ Approve refactoring, then /coding — or skip with /testing` |
| No Testing Report | `→ Next: run /testing` |
| All reports present | `→ Pipeline complete. Run /archive <name>` |

---

## Requirements

- Claude Code CLI v2.1.0+
- Node.js (for the Stop hook script)
