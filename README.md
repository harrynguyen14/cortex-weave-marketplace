# CortexWeave Plugin for Claude Code

A Claude Code plugin that turns Claude into a full multi-agent software development pipeline. Describe your project, and CortexWeave spawns specialized AI agents to design, build, test, and document it.

## Install

```bash
# From the CortexWeave repo root
cp -r plugin/ /path/to/your/project/.claude-plugin/cortexweave/

# Or install globally
cp -r plugin/ ~/.claude/plugins/cortexweave/
```

Or add this repo as a Claude Code plugin directly:
1. Open Claude Code settings
2. Add plugin path: `path/to/CortexWeave/plugin`

## Skills (slash commands)

| Command | What it does |
|---------|-------------|
| `/cortexweave:weave-run` | Full pipeline — Discussion → Plan → Build → Test → Report |
| `/cortexweave:weave-discuss` | Phase 1 only — structured requirement gathering → `conversation.md` |
| `/cortexweave:weave-plan` | Phase 2 only — decompose brief into task list → `plan.md` |
| `/cortexweave:weave-review` | On-demand architecture audit → `audit_report.md` |
| `/cortexweave:weave-skill` | Generate a new role skill file for the inventory |

## Agents

The plugin registers these agents that the orchestrator can delegate to:

| Agent | Role |
|-------|------|
| `weave-discussion` | Requirement gathering interviewer |
| `weave-planner` | Vertical-slice task decomposer |
| `weave-orchestrator` | Parallel task executor, skill selector |
| `weave-auditor` | Background architecture guard (Haiku model) |
| `weave-tester` | Test generator and executor |
| `weave-reporter` | Documentation synthesizer |
| `weave-dynamic` | Fallback sub-agent for any domain |

## Pipeline Flow

```
You: /cortexweave:weave-run

→ weave-discuss    asks 3-5 questions, writes conversation.md
→ weave-plan       decomposes into vertical slices, writes plan.md
                   [USER APPROVES PLAN]
→ weave-orchestrator  spawns sub-agents per task (up to 5 parallel)
   ↳ weave-dynamic  (architect task)
   ↳ weave-dynamic  (backend task)     ← skill auto-selected per task
   ↳ weave-dynamic  (frontend task)
→ weave-tester     tests all output against api_spec.md
→ weave-reporter   writes reports/README.md + reports/doc.md
```

## Workspace Files

Each pipeline run produces these files in your project directory:

| File | Written by | Contains |
|------|-----------|---------|
| `conversation.md` | Discussion phase | Requirements source of truth |
| `plan.md` | Planning phase | Task list JSON |
| `architecture_design.md` | Architect sub-agent | Module boundaries |
| `api_spec.md` | Architect sub-agent | Endpoint contracts |
| `todo.md` | Orchestrator | Live task status |
| `memory_[agent].md` | Each sub-agent | Working memory |
| `testing.md` | Tester phase | Test results |
| `audit_report.md` | Auditor | Architecture violations |
| `reports/README.md` | Report phase | User-facing docs |
| `reports/doc.md` | Report phase | Technical documentation |

## Skill Inventory

The plugin uses the CortexWeave skill system — 33+ role skills are available for automatic assignment:

**Architecture & Design:** architect, api-designer, ui-designer

**Backend:** backend, python-pro, golang-pro, django-developer, java-architect, rust-engineer

**Frontend:** frontend, react-specialist, typescript-pro, nextjs-developer, fullstack-developer

**DevOps:** devops, docker-expert, kubernetes-specialist, terraform-engineer, build-engineer

**Quality:** tester, qa-expert, code-reviewer, security_auditor, penetration-tester

**Data & AI:** data-scientist, ai-engineer, mlops-engineer, data-engineer

**Specialized:** blockchain-developer, git-workflow-manager, documentation-engineer, refactoring-specialist, tech_writer

Add your own with `/cortexweave:weave-skill`.

## Architecture Rules

The AuditorAgent enforces these during every pipeline run:

- **AR001 CRITICAL** — `weave_core` never imports from `weave_runtime`
- **AR002 CRITICAL** — Sub-agents (F1) never spawn other agents
- **AR003 CRITICAL** — Database access only through port interfaces
- **AR004 WARN** — No cross-agent module imports
- **AR005 WARN** — No mutable state shared between pipeline phases

## Token Budget

Default: 200,000 tokens per pipeline run.
- Warning at 80% — you'll be asked whether to continue
- Hard stop at 100% — no new agents spawned

## Requirements

- Claude Code with agent support
- Python 3.12+ (for the CortexWeave runtime)
- Docker + Docker Compose (for test execution)
