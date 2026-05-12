# Implementer Subagent Prompt Template

Use this template when the Orchestrator spawns an implementer via `weave-dynamic`.

The Orchestrator fills in all `[placeholders]` before dispatching — subagent receives a complete, self-contained prompt.

```
Agent tool:
  subagent_type: cortexweave:weave-dynamic
  description: "Implement [task_id]: [task_title]"
  prompt: |
    [Role skill content from agents/roles/[role].md — prepend here]

    ---

    ## Architecture Context

    [Full content of architecture_design.md — paste directly, do not make subagent read it]

    ---

    ## API Contract

    [Full content of api_spec.md — paste directly, do not make subagent read it]

    ---

    ## Your Task

    **[task_id]: [task_title]**

    [Full task description including acceptance criteria — paste from plan.md]

    Skill needed: [task.skill_hint]

    Write output to: output/[relevant_dir]/
    Write memory to: memory_[task_id].md
```

## Rules for the Orchestrator

- Always paste full content of architecture_design.md and api_spec.md — never tell subagent to read them
- Always paste full task description — never tell subagent to read plan.md
- Prepend the matched role skill above the architecture context
- If architecture_design.md or api_spec.md do not exist yet (Phase 1), omit those sections
