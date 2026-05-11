---
name: weave-plan
description: Use when a conversation.md or project brief exists and needs to be decomposed into a dependency-ordered task list — not for sprint planning or roadmap reviews of existing projects
---

# CortexWeave — Planning Phase

Decomposes a project brief into a vertical-slice task list that the Orchestrator can execute in parallel.

## Input

- `conversation.md` if it exists — use it directly
- Otherwise ask the user for a 2-3 sentence project description

## Vertical Slicing Rules

Each task = one thin slice with standalone value. Max size: **L (8-16h)**. Split anything XL before writing the plan.

Sizing: XS < 2h · S 2-4h · M 4-8h · L 8-16h · XL = split

Dependency rules:
1. Architecture tasks first — `depends_on: []`
2. Backend before frontend that calls it
3. DevOps runs parallel to implementation
4. Tests depend on what they test
5. Report depends on tests passing

## Output — plan.md

```json
{
  "summary": "2-3 sentence overview",
  "tasks": [
    {
      "id": "T001",
      "title": "Verb-first title",
      "skill_hint": "specific expertise (e.g. 'FastAPI backend with JWT auth')",
      "priority": "high|medium|low",
      "depends_on": [],
      "description": "What to build + acceptance criteria"
    }
  ]
}
```

## Quality Check

- [ ] Every `skill_hint` is specific (not "backend" — write "FastAPI + PostgreSQL auth service")
- [ ] No XL tasks
- [ ] No dependency cycles
- [ ] At least 2 tasks with `depends_on: []`

## Done Signal

Write `plan.md`, then report: total tasks, tasks that can start immediately, peak parallelism. Ask user to approve before Orchestrator runs.
