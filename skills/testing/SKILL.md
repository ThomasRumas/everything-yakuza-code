---
name: testing
description: Use this skill to generate tests after coding (and optionally refactoring). Invokes the Testing agent to map every acceptance criterion to at least one test case, covering edge cases and failure modes. Appends a Testing Report to spec.md. Never modifies production code.
origin: everything-yakuza-code
---

# Testing Skill

## When to Use

Use this skill when:
- The `## Execution Report` section exists in `.agents/spec.md`
- You want automated tests generated from the acceptance criteria

## How to Invoke

Invoke the `testing` agent to generate tests based on `.agents/spec.md`.

## Workflow

1. Read `.agents/spec.md`. If the file does not exist, stop and tell the user to run `/orchestrator` first.
2. Delegate to the `testing` agent to generate tests derived from:
   - `## Business Acceptance Criteria`
   - `## Edge Cases`
   - `# Testing Strategy`
3. The agent must not modify any production code — it only produces test files and test plans.
4. Every acceptance criterion must map to at least one test case.
5. Once complete, append the following section to `.agents/spec.md`:

```
## Testing Report
<Testing Report output here>
```

## Output

Test files covering unit, integration, and edge cases, plus an updated `.agents/spec.md` with a `## Testing Report` section.
