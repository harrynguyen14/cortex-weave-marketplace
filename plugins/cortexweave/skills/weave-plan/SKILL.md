---
name: weave-plan
description: Use when a conversation.md or project brief exists and needs to be decomposed into a dependency-ordered task list — not for sprint planning or roadmap reviews of existing projects
---

# CortexWeave — Planning Phase

Decomposes a project brief into phases with contract-first ordering.

## Input

- `conversation.md` if it exists — use it directly
- Otherwise ask for a 2-3 sentence project description

## Contract-First Rule

**Backend must always precede Frontend and Docker.**
Phase order is enforced:
1. Architecture + `api_spec.md` (contract between all layers)
2. Backend (must write `api_spec.md` before finishing)
3. Frontend (must read `api_spec.md` before writing any API call)
4. Docker/DevOps (must read `api_spec.md` for ports and env vars)
5. Tests → Report

This prevents the most common failures: CORS errors, wrong endpoints, Docker port mismatches.

## Phase Rules

- **Max 8 phases** — merge related tasks if more
- Each phase has one clear deliverable the user can review
- Tasks within a phase run in parallel; phases run sequentially
- After each phase completes, ask user before starting next

```
Phase [N] complete: [deliverable]
Continue to Phase [N+1]: [what's next]? (yes / adjust / abort)
```

## Sizing

XS < 2h · S 2-4h · M 4-8h · L 8-16h · XL = split before planning

## Output — plan.md

```json
{
  "summary": "2-3 sentence overview",
  "phases": [
    {
      "phase": 1,
      "name": "Phase 1 — Architecture & API Contract",
      "deliverable": "architecture_design.md + api_spec.md written and reviewed",
      "tasks": [
        {
          "id": "T001",
          "title": "Design system architecture and API contract",
          "skill_hint": "architect — system design with explicit API contract (api_spec.md)",
          "size": "M",
          "priority": "high",
          "depends_on": [],
          "description": "Write architecture_design.md and api_spec.md. api_spec.md must include: all endpoints, request/response schemas, auth headers, error codes, and CORS policy.",
          "outputs": ["output/architecture_design.md", "output/api_spec.md"]
        }
      ]
    }
  ]
}
```

## Quality Check

- [ ] Phase 1 always produces `api_spec.md`
- [ ] Every backend task lists `api_spec.md` as input
- [ ] Every frontend task lists `api_spec.md` as input and has `depends_on` backend phase
- [ ] Every Docker task lists `api_spec.md` as input for ports/env
- [ ] No XL tasks
- [ ] No dependency cycles
- [ ] Total phases ≤ 8
- [ ] Each task has `outputs` field listing files it writes

## Done Signal

Write `plan.md`, present phase breakdown, and ask:

```
Plan ready — [N] phases:
  Phase 1: [name] ([X] tasks) → delivers: [contract files]
  Phase 2: [name] ([X] tasks)
  ...

Approve to run Phase 1? (yes / adjust / abort)
```

**Do NOT start execution until user explicitly approves.**
