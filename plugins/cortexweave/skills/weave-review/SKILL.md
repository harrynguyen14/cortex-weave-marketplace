---
name: weave-review
description: Use when implementation is complete and you need to check for architecture boundary violations, direct database access, or illegal cross-agent imports before running tests
---

# CortexWeave — Architecture Review

On-demand audit of all output files against AR001–AR005. Produces `audit_report.md`.

## Rules to Check

| Rule | Severity | Check |
|------|----------|-------|
| AR001 | CRITICAL | `weave_core` imports from `weave_runtime` |
| AR002 | CRITICAL | F1 sub-agent calls any agent-spawning function |
| AR003 | CRITICAL | Agent accesses DB directly (not through a port) |
| AR004 | WARN | Agent imports from another agent's module |
| AR005 | WARN | Pipeline phases share mutable state |

## How to Check

- **AR001** — grep `from weave_runtime` inside `weave_core/`
- **AR002** — grep `spawn_agent`, `SubAgent(`, `Agent(` inside F1 agent files
- **AR003** — grep `db.execute`, `session.query`, `db.cursor` outside `ports/` and `repositories/`
- **AR004** — grep cross-agent imports (e.g. `from weave_backend import` inside `weave_frontend`)
- **AR005** — grep module-level globals mutated across phases

## Violation Format

```
🚨 CRITICAL [AR001] output/backend/services/auth.py:12
  weave_core imports from weave_runtime
  Fix: use port interface at weave_core/ports/auth_port.py

⚠️  WARN [AR004] output/frontend/api/client.py:3
  Direct import from weave_backend
  Fix: call via HTTP using api_spec.md contract
```

## Output — audit_report.md

```markdown
# Architecture Review — [date]
## Result: PASS / FAIL (N critical)
### Critical Violations · Warnings · Clean Checks
```

## Done Signal

Tell the user: critical count, files to fix, and whether the codebase is safe to hand to the TesterAgent. If CRITICAL violations exist, recommend fixing before tests — the violations are likely the root cause of any test failures.
