---
name: weave-extend
description: Use when an existing app needs a new feature added — not for bug fixes or full rebuilds
---

# CortexWeave — Feature Extension Pipeline

Adds a new feature to an existing codebase without breaking existing functionality.

## Step 1 — Understand the Request

Ask the user:
1. What feature do you want to add? (describe behavior, not implementation)
2. Which part of the app is affected? (Frontend / Backend / Both / Docker)
3. Any constraints? (must use existing auth, must not change DB schema, etc.)

## Step 2 — Scan Existing Codebase

Before planning anything:
- Read `api_spec.md` to understand existing contracts
- Read `architecture_design.md` to understand module boundaries
- Read relevant source files in the affected area
- Run existing tests to establish a baseline

**Goal: understand what already exists before deciding what to add.**

## Step 3 — Research (if needed)

If the feature requires an unfamiliar library or integration, search online for:
- Official documentation and examples
- Known pitfalls or breaking changes
- Compatibility with the existing stack

## Step 4 — Design the Extension

Produce a mini-spec:
```
Feature: [name]
Affected layers: [Frontend / Backend / Docker]

Backend changes:
  - New endpoints: [list — will be added to api_spec.md]
  - DB changes: [new tables/columns or "none"]

Frontend changes:
  - New pages/components: [list]
  - API calls added: [list]

Docker changes:
  - New env vars: [list or "none"]
  - New ports: [list or "none"]
```

Show to user and ask:
> "Does this design match what you want? (yes / adjust / abort)"

**Do NOT write any code until user confirms the design.**

## Step 5 — Update api_spec.md First

If the feature adds backend endpoints:
1. Add new endpoints to `api_spec.md` before writing implementation
2. Show the additions to the user
3. Only then implement backend, then frontend

This prevents the Frontend from calling endpoints that don't exist yet.

## Step 6 — Implement in Order

Always implement in this order:
1. Backend (new endpoints, business logic)
2. Frontend (consume new endpoints from `api_spec.md`)
3. Docker (add env vars/ports if needed)
4. Tests (cover the new feature)

After each layer, run tests before moving to the next.

## Step 7 — Verify

```
Feature implemented: [name]
Files changed: [list]
New tests: [X added, Y passed]
api_spec.md: [updated / unchanged]
```

## Rules

- Never break existing endpoints — only add new ones
- Never remove existing DB columns — only add new ones
- If a change requires modifying an existing contract, show the user the diff and get explicit approval
- Keep changes scoped to the feature — no opportunistic refactoring
