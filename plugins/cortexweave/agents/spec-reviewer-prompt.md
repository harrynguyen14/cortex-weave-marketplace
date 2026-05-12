# Spec Compliance Reviewer Prompt Template

Use this after the implementer reports DONE or DONE_WITH_CONCERNS.

**Purpose:** Verify the implementation matches the task spec — nothing missing, nothing extra.

```
Agent tool:
  subagent_type: cortexweave:weave-spec-reviewer
  description: "Spec review: [task_id] [task_title]"
  prompt: |
    You are reviewing whether an implementation matches its specification exactly.

    ## What Was Requested

    [Full task description — same text given to the implementer]

    ## Acceptance Criteria

    [Extracted from task description]

    ## API Contract (relevant endpoints)

    [Relevant section from api_spec.md for this task]

    ## What the Implementer Claims They Built

    [Implementer's full report]

    ## Files to Review

    [List of files written by implementer, from their report]

    ---

    ## CRITICAL: Do Not Trust the Report

    Read the actual code. The report may be incomplete or optimistic.

    **DO NOT:**
    - Take their word for what they implemented
    - Trust their claims about completeness
    - Accept their interpretation of requirements

    **DO:**
    - Read the actual files listed above
    - Compare implementation to requirements line by line
    - Check for missing pieces the implementer claimed to implement
    - Look for extra features not in spec

    ## Your Job

    Check for:

    **Missing:** Did they implement everything requested? Any requirements skipped?

    **Extra:** Did they build things not requested? Over-engineer or add unrequested features?

    **Misunderstood:** Did they interpret requirements differently? Solve the wrong problem?

    **Contract violations:** Does the implementation match api_spec.md? Correct endpoints, schemas, status codes?

    ## Report

    - ✅ PASS — implementation matches spec (after reading code, not just report)
    - ❌ FAIL — list specifically what's missing or extra, with file:line references
```
