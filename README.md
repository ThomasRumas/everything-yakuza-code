# Everything Yakuza Code

Agent-based software development workflow plugin for Claude Code.

A structured pipeline of specialized agents — plan, code, refactor, test — with a guided hook that tells you what to do next after every step.

---

## Install

### From Marketplace

```bash
# Add marketplace
/plugin marketplace add https://github.com/thomasrumas/everything-yakuza-code

# Install plugin
/plugin install eyc@eyc
```

---

## The Pipeline

```
claude --agent eyc:orchestrator "feature description"
       ↓
  @eyc:architect → produces plan
       ↓
   [user confirms plan]        ← human gate #1
       ↓
  @eyc:coding → executes plan
       ↓
  @eyc:refactor → reviews code
       ↓
   [issues?]
    yes → [user approves fixes] ← human gate #2
           ↓
         @coding (apply fixes)
    no  → continue
       ↓
  @eyc:testing → generates tests
       ↓
  @eyc:archive → preserves spec
```

After every Claude response, a hook reads `.agents/spec.md` and prints the suggested next step — so you never have to remember where you are in the pipeline.

---

## Orchestrator (Recommended)

The **orchestrator** agent runs the full pipeline automatically. You invoke it once with your feature description, and it delegates to each specialized agent in sequence.

### Usage

```bash
# Start a session with the orchestrator as main agent
claude --agent eyc:orchestrator

# Or set it as default in .claude/settings.json
{
  "agent": "eyc:orchestrator"
}
```

Then describe your feature:

```
Build a user authentication system with JWT tokens and refresh token rotation
```

### What happens automatically

1. **Architect** produces the plan → orchestrator presents it to you
2. **You confirm** the plan (only human gate #1)
3. **Coding** executes the plan
4. **Refactor** reviews the code
5. If issues found → orchestrator presents them → **you decide** whether to apply fixes (human gate #2)
6. **Testing** generates tests
7. **Archive** preserves the spec

### Human gates

| Gate | When | What you decide |
|------|------|-----------------|
| Plan approval | After architect finishes | Confirm the plan is correct before coding starts |
| Refactor fixes | Only if refactor finds issues | Whether to apply the suggested improvements |

If no refactoring issues are found, the pipeline continues straight to testing without interruption.

---

## Individual Agents

You can still invoke agents individually outside the orchestrator when needed — for example, to re-run just the testing stage or to iterate on the plan.

Invoke agents with `@eyc:agent-name` in Claude Code. Each has a strict role:

Invoke agents with `@eyc:agent-name` in Claude Code. Each has a strict role:

| Agent | Invocation | Role |
|---|---|---|
| **Orchestrator** | `claude --agent eyc:orchestrator` | Runs the full pipeline — delegates to all agents below |
| **Architect** | `@eyc:architect "feature"` | Produces full implementation plan → `.agents/spec.md` |
| **Coding** | `@eyc:coding` | Executes the plan task by task. Stops if anything ambiguous |
| **Refactor** | `@eyc:refactor` | Proposes safe, behavior-preserving improvements only |
| **Testing** | `@eyc:testing` | Generates tests from acceptance criteria. Never touches production code |
| **Archive** | `@eyc:archive <name>` | Moves spec to `.agents/archived/<name>.md` when done |

### How Custom Agents Work in Claude Code

Claude Code agents are Markdown files with YAML frontmatter. They live in the `agents/` directory of a plugin. Each agent defines:

- **name** — identifier used with `@eyc:agent-name`
- **description** — what the agent does (shown in `/agents` list)
- **tools** — which tools the agent can use (Read, Write, Bash, Grep, etc.)
- **model** — `inherit` uses the current model, or specify one
- **permissionMode** — `acceptEdits` auto-approves file edits
- **memory** — `project` enables project-scoped memory

When you type `@eyc:architect "build a login page"`, Claude Code loads the architect agent's system prompt and restricts its capabilities to the declared tools.

---

## How `.agents/spec.md` works

All agents share a single file: `.agents/spec.md`.

Each pipeline stage appends its report to that file:

```
spec.md
├── # Business Specification       ← architect writes
├── # Technical Specification      ← architect writes
├── # Implementation Plan          ← architect writes
├── ## Execution Report            ← coding appends
├── ## Refactoring Report          ← refactor appends
└── ## Testing Report              ← testing appends
```

The pipeline hook reads which sections exist and guides you to the next step.

---

## Pipeline Hook

After every Claude response, the hook checks `.agents/spec.md` state:

| State | Suggestion |
|---|---|
| No spec.md | *(silent)* |
| No Execution Report | `→ Next: run @eyc:coding` |
| No Refactoring Report | `→ Next: run @eyc:refactor` |
| Refactoring pending approval | `→ Approve refactoring, then @eyc:coding — or skip with @eyc:testing` |
| No Testing Report | `→ Next: run @eyc:testing` |
| All reports present | `→ Pipeline complete. Run @eyc:archive <name>` |

---

## Skills

The plugin ships these skills (auto-invoked by Claude based on context):

| Skill | Purpose |
|---|---|
| **architecture-decision-record** | Captures architectural decisions as structured ADR documents |
| **playwright-cli** | Browser automation for frontend verification |

---

## Frontend Verification

The `@coding` agent uses `playwright-cli` for frontend verification. The skill is bundled with this plugin (from [microsoft/playwright-cli](https://github.com/microsoft/playwright-cli)).

When implementing UI tasks, the coding agent will automatically open a browser, navigate to your local dev server, and verify the rendered output matches expectations.

```bash
# Requires playwright-cli installed
npm install -g @playwright/cli@latest
```

---

## Architecture Decision Records

The `@eyc:architect` agent can propose creating ADRs when it detects significant architectural decisions. It will ask for your agreement before writing. ADRs are stored in `docs/adr/` using the Nygard format.

---

## Requirements

- Claude Code CLI v2.1.0+
- Playwright CLI (optional, for frontend verification)
