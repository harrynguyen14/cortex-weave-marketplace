# CortexWeave Architecture Rules

These rules are enforced by the AuditorAgent (weave-auditor) during implementation and QA phases.

## Dependency Direction

`weave_core` → nothing (no upstream imports)
`weave_runtime` → `weave_core`, `weave_skills`, `weave_memory`
`weave_skills` → `weave_core`
`weave_memory` → `weave_core`

**Never reverse this direction.** `weave_core` must be a pure domain layer with zero runtime dependencies.

## Agent Depth Limit

- F0 = Orchestrator (spawns agents)
- F1 = Sub-agents (execute tasks, no spawning)

F1 agents MUST NOT call any agent factory, spawn function, or agent SDK. If a task is too large for one agent, the Orchestrator splits it — not the sub-agent.

## Database Access

Agents access data only through port interfaces defined in `weave_core/ports/`. Direct database connections (`psycopg2`, raw SQL, `db.execute`) are forbidden outside repository implementations.

## Module Isolation

Each agent module is an island. Cross-agent imports are forbidden. Agents communicate through:
- Workspace files (`conversation.md`, `plan.md`, `todo.md`, etc.)
- The port/interface layer
- Explicit task handoff via the Orchestrator

## Mutable State

Pipeline phases must not share mutable state. Each phase reads its inputs from workspace files and writes its outputs to workspace files. No module-level globals modified across phases.
