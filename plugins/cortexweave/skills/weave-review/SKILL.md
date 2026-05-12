---
name: weave-review
description: Use when implementation is complete and you need to check for architecture boundary violations, integration contract mismatches, and Docker configuration errors before running tests
---

# CortexWeave — Architecture Review

Audits all output files against architecture rules and integration contracts. Produces `audit_report.md`.

## Rules to Check

| Rule | Severity | Check |
|------|----------|-------|
| AR001 | CRITICAL | `weave_core` imports from `weave_runtime` |
| AR002 | CRITICAL | F1 sub-agent calls any agent-spawning function |
| AR003 | CRITICAL | Agent accesses DB directly (not through a port) |
| AR004 | CRITICAL | Frontend calls an endpoint NOT in `api_spec.md` |
| AR005 | CRITICAL | Docker exposes a port NOT declared in `api_spec.md` |
| AR006 | WARN | Frontend hardcodes a base URL instead of using env var |
| AR007 | WARN | Backend missing CORS header for frontend origin |
| AR008 | WARN | Agent imports from another agent's module |

## How to Check

- **AR001** — grep `from weave_runtime` inside `weave_core/`
- **AR002** — grep `spawn_agent`, `SubAgent(`, `Agent(` inside F1 agent files
- **AR003** — grep `db.execute`, `session.query`, `db.cursor` outside `ports/` and `repositories/`
- **AR004** — diff frontend API calls against `api_spec.md` endpoints
- **AR005** — diff `docker-compose.yml` / `Dockerfile` EXPOSE against `api_spec.md` ports
- **AR006** — grep hardcoded `http://localhost` or `http://0.0.0.0` in frontend source
- **AR007** — grep CORS config in backend, verify allowed origins include frontend origin
- **AR008** — grep cross-agent imports

## Violation Format

```
CRITICAL [AR004] output/frontend/api/client.py:14
  Calls POST /api/v1/login — not in api_spec.md
  Fix: use endpoint POST /auth/login defined in api_spec.md

WARN [AR006] output/frontend/src/api.ts:3
  Hardcoded URL: http://localhost:8000
  Fix: use process.env.REACT_APP_API_URL
```

## Output — audit_report.md

```markdown
# Architecture Review — [date]
## Result: PASS / FAIL (N critical)
### Critical Violations
### Warnings
### Clean Checks
```

## Done Signal

Report: critical count, files to fix, safe to proceed to tests or not.
If CRITICAL violations exist — fix before running tests, they are the root cause of failures.
