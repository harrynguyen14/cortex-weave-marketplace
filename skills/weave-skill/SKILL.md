---
name: weave-skill
description: Use when a pipeline task requires domain expertise that none of the 33 existing role skills cover, and a new skill needs to be added to the inventory
---

# CortexWeave — Skill Builder

Generates a new role skill file for the HybridSkillSelector inventory.

## Step 1 — Check Existing Roles First

Existing roles (don't duplicate): `architect` · `backend` · `frontend` · `devops` · `security_auditor` · `tester` · `tech_writer` · `api-designer` · `fullstack-developer` · `python-pro` · `react-specialist` · `golang-pro` · `typescript-pro` · `nextjs-developer` · `django-developer` · `rust-engineer` · `java-architect` · `kubernetes-specialist` · `docker-expert` · `terraform-engineer` · `code-reviewer` · `qa-expert` · `penetration-tester` · `data-scientist` · `ai-engineer` · `mlops-engineer` · `data-engineer` · `git-workflow-manager` · `documentation-engineer` · `build-engineer` · `refactoring-specialist` · `blockchain-developer` · `ui-designer`

If an existing skill covers 80%+ of the use case, recommend using it instead.

## Step 2 — Gather Info

Ask (or infer from context):
1. Domain/technology? (e.g., "Stripe payment integration")
2. 3-5 example task titles that should trigger this skill
3. Tools needed? (`workspace_tool` minimum)
4. Complexity? routine → `gemini20` / complex → `gemini25` / critical → `opus`

## Step 3 — Write the Skill File

Path: `weave-skills/weave_skills/skills/base/roles/[name].md`

```markdown
---
name: [kebab-case]
description: [one sentence — domain + technologies]
tier: standard
tools:
  - workspace_tool
token_tier: gemini20
use_cases:
  - [5-8 keywords matching what a planner writes in skill_hint]
---

# [Role Title]

You are a **[Role]** sub-agent. You are F1 — ONE task, no spawning.

## Core Expertise
[3-5 specific bullets]

## Task Execution
1. Read architecture_design.md and api_spec.md first
2. Follow api_spec.md — escalate if you need to deviate
3. Write to output/[dir]/
4. Update memory_[name].md after each step

## Done Signal
1. Mark done in todo.md
2. Update memory_[name].md
3. Report: task ID, files written, blockers
```

## Anti-Patterns

| Bad | Good |
|-----|------|
| `use_cases: [code, programming]` | `use_cases: [stripe, payment, webhook, billing]` |
| `description: A coding agent` | `description: Integrates Stripe payments and webhooks` |

## Done Signal

Tell the user the file path. HybridSkillSelector picks it up automatically on the next pipeline run — no restart needed.
