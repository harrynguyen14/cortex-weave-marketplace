# CortexWeave — Multi-Agent Pipeline

CortexWeave is a 5-phase multi-agent orchestration system. When working inside this project, follow these rules.

## Pipeline Phases

```
Phase 1 — Discussion   : DiscussionAgent gathers requirements → writes conversation.md
Phase 2 — Planning     : PlannerAgent decomposes → writes plan.md (JSON task list)
Phase 3 — Implementation: OrchestratorAgent spawns DynamicSubAgents per task (parallel)
Phase 4 — QA           : TesterAgent generates & runs tests → writes testing.md
Phase 5 — Report       : ReportAgent synthesizes → writes README.md + doc.md
[Background] Auditor   : AuditorAgent checks architecture violations every 30s
```

## Task JSON Schema (plan.md)

```json
{
  "summary": "2-3 sentence overview",
  "tasks": [
    {
      "id": "T001",
      "title": "Action-oriented title",
      "skill_hint": "specific expertise description",
      "priority": "high|medium|low",
      "depends_on": [],
      "description": "What to build + acceptance criteria"
    }
  ]
}
```

## Skill System

Skills live in `weave-skills/weave_skills/skills/base/`:
- `roles/`      — 33 sub-agent role skills (backend, frontend, architect, etc.)
- `process/`    — pipeline process skills (plan)
- `quality/`    — quality review skills
- `guidelines/` — coding_guidelines (prepended to every agent), skill_builder

**HybridSkillSelector** picks skills by matching `task.title + task.description` against skill `use_cases` keywords, then falls back to LLM selection.

**SkillComposer** combines selected skills into one composed prompt — always prepends `coding-guidelines`.

## Workspace File Conventions

| File | Owner | Purpose |
|------|-------|---------|
| `conversation.md` | DiscussionAgent | Requirements source of truth |
| `plan.md` | PlannerAgent | Task list (JSON) |
| `architecture_design.md` | Architect sub-agent | Module boundaries + tech stack |
| `api_spec.md` | Architect sub-agent | Endpoint contracts |
| `todo.md` | Orchestrator | Task status (Redis → export) |
| `memory_[agent].md` | Each sub-agent | Working memory per agent |
| `testing.md` | TesterAgent | Test results |
| `README.md` | ReportAgent | User-facing docs |

## Architecture Rules (enforced by AuditorAgent)

- AR001 CRITICAL: `weave_core` must NOT import `weave_runtime`
- AR002 CRITICAL: Sub-agents (F1) must NOT spawn other agents
- AR003 CRITICAL: Agents access DB only through ports, never directly
- AR004 WARN: Agents must NOT import from other agent modules directly
- AR005 WARN: Pipeline phases must NOT share mutable state

## Key Package Locations

```
weave-core/      — Domain entities (Task, Skill, AgentConfig, Event)
weave-memory/    — 4-store memory (Redis, Qdrant, SQLite episodic, SQLite bridge)
weave-skills/    — Skill loading, inventory, HybridSkillSelector, SkillComposer
weave-runtime/   — Agents, pipeline phases, factory, DI container
weave-sandbox/   — FastMCP sandbox for test execution
weave-session/   — Session + pipeline state
weave-vault/     — Credential management
```

## Token Tiers

| Tier | Model | Use for |
|------|-------|---------|
| `gemini20` | Gemini 2.0 Flash | Routine implementation tasks |
| `gemini25` | Gemini 2.5 Pro | Architecture, security, complex reasoning |
| `opus` | Claude Opus | Highest-stakes decisions, respawn escalation |

Token budget default: 200k per pipeline. Warn at 80%, hard stop at 100%.
