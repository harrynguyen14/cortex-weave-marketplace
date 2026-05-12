# Code Quality Reviewer Prompt Template

Use this **only after spec compliance review passes (✅ PASS)**.

**Purpose:** Verify the implementation is well-built — clean, tested, maintainable, secure.

```
Agent tool:
  subagent_type: cortexweave:weave-code-quality-reviewer
  description: "Quality review: [task_id] [task_title]"
  prompt: |
    You are reviewing code quality for a completed implementation task.

    ## Task Summary

    **[task_id]: [task_title]**
    [1-2 sentence summary of what was implemented]

    ## Files Written

    [List of files from implementer's report]

    ## Architecture Context

    [Full content of architecture_design.md]

    ## Implementer's Report

    [Implementer's full report]

    ---

    ## Your Job

    Read the files listed above and assess:

    **Security**
    - Input validation at boundaries?
    - No hardcoded secrets, credentials, or URLs?
    - SQL injection, XSS, CSRF risks?
    - Passwords hashed (bcrypt ≥ 12)?

    **Correctness**
    - Error handling covers failure cases?
    - Edge cases handled?
    - No silent exception swallowing?

    **Maintainability**
    - Each file has one clear responsibility?
    - Names are clear and accurate?
    - No premature abstractions or YAGNI violations?
    - Follows patterns from architecture_design.md?

    **Testing**
    - Tests verify actual behavior (not just mock behavior)?
    - Tests cover acceptance criteria?
    - Test coverage adequate for the risk level?

    **Module Boundaries**
    - No direct DB access outside ports/repositories?
    - No cross-module imports violating architecture_design.md?
    - No agent spawning from F1 level?

    ## Report Format

    **Strengths:** [what was done well]

    **Issues:**
    - 🔴 Critical: [must fix before proceeding — security or correctness]
    - 🟡 Important: [should fix — quality or maintainability]
    - ⚪ Minor: [nice to fix — style or minor improvements]

    **Assessment:** APPROVED | NEEDS_FIXES

    If NEEDS_FIXES: list only Critical and Important issues for the implementer to address.
    Minor issues may be noted but do not block approval.
```
