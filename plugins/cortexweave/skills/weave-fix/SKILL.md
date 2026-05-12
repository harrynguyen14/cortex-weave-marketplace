---
name: weave-fix
description: Use when an existing app has bugs, errors, or broken functionality that needs to be diagnosed and fixed — not for adding new features
---

# CortexWeave — Bug Fix Pipeline

Diagnoses and fixes bugs in an existing codebase using a structured scientific method.

## Step 1 — Gather Bug Context

Ask the user (all required before proceeding):
1. What is the error? (paste error message or describe behavior)
2. Where does it occur? (which page, endpoint, or step to reproduce)
3. What was the expected behavior?
4. Which files or modules are likely involved? (or "unknown")

## Step 2 — Read Before Touching

Before any fix attempt:
- Read the files involved
- Read `api_spec.md` if the bug involves Frontend↔Backend communication
- Read `architecture_design.md` if the bug involves module boundaries
- Run existing tests to establish a baseline: `python -m pytest` or equivalent

**Never edit code before understanding the root cause.**

## Step 3 — Diagnose (Scientific Method)

Form a hypothesis:
```
Symptom:    [what the user sees]
Hypothesis: [suspected root cause]
Evidence:   [file:line that supports hypothesis]
```

If multiple hypotheses, rank by likelihood and test the most probable first.

## Step 4 — Confirm Fix Plan with User

Before making any changes, show:
```
Root cause: [explanation]
Files to change:
  - [file:line] — [what will change]

Side effects: [any other behavior that may be affected]
Apply fix? (yes / try different approach / abort)
```

**Do NOT edit files until user confirms.**

## Step 5 — Apply Fix

- Make the minimal change that fixes the root cause
- Do not refactor surrounding code
- Do not add features while fixing
- After fix: run tests and show results

## Step 6 — Verify

```
Fix applied to: [files changed]
Tests: [X passed / Y failed]
Behavior: [confirm the symptom is gone]
```

If tests still fail, return to Step 3 with new evidence.

## Rules

- One root cause at a time — do not fix multiple unrelated bugs in one pass
- If the bug is in the Frontend↔Backend contract, check `api_spec.md` first — mismatched endpoints are the #1 cause
- If Docker fails to start, check env vars and exposed ports against `api_spec.md`
- Never use `rm -rf`, `drop database`, or destructive commands to "reset" the problem
