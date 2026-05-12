---
name: weave-run
description: Use when the user wants to build a complete software project from scratch and have AI agents handle design, implementation, testing, and documentation end-to-end
---

# CortexWeave — Full Pipeline

Runs all phases: Discussion → Planning → Implementation → QA → Report.

## Step 1 — Gather Context

Ask the user (required before proceeding):
1. What are you building? (problem + who uses it)
2. Tech stack preference? (or "let the architect decide")
3. Output workspace path? (default: `./output/`)

## Step 2 — Research Before Advising

Search online for:
- Best practices and common architectures for this type of project
- Recommended libraries/frameworks for the stated tech stack
- Known pitfalls for this problem domain

Use findings to give informed recommendations, not generic suggestions.

## Step 3 — Advise and Confirm

Present to user:
- Recommended architecture + rationale from research
- Suggested tech stack with tradeoffs
- Estimated scope

Show summary and **wait for explicit "yes"** before proceeding:

```
Project: [summary]
Stack:   [recommendation]
Output:  [path]

Phases: Discussion → Planning → Implementation → QA → Report
Confirm? (yes/no)
```

## Step 4 — Discussion Phase

Run discussion phase. After it completes, show summary of `conversation.md` and ask:
> "Does this capture your requirements? (yes / edit / abort)"

**Do NOT start planning until user confirms.**

## Step 5 — Planning Phase

Run planning phase. After `plan.md` is written, show phase breakdown and ask:
> "Approve Phase 1 to start? (yes / adjust / abort)"

**Do NOT execute until user approves.**

## Step 6 — Execute Phases with Confirmation

Run phases sequentially. After each phase:

```
Phase [N] complete: [deliverable]
Continue to Phase [N+1]: [what's next]? (yes / adjust / abort)
```

**Do NOT start next phase without explicit confirm.**

## Step 7 — Final Summary

Report: `reports/README.md` location, QA pass/fail count, any architecture violations.

## Rules

- Never skip QA
- Warn when token budget hits 80%
- Never silently retry a phase failure more than 3 times — surface the error and ask user
