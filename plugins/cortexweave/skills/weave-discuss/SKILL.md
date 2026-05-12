---
name: weave-discuss
description: Use when requirements are missing, vague, or less than a paragraph — not when the user says "let's build" or "start coding" with a clear enough spec already stated
---

# CortexWeave — Discussion Phase

Extracts complete requirements through a structured interview. Writes `conversation.md`.

## Research First

Before asking questions, search online for:
- Common architectures for this problem domain
- Recommended libraries/frameworks for stated tech stack
- Known pitfalls from similar projects

Use findings to give grounded recommendations during the interview.

## Capture Rubric

Must answer ALL before finishing:

| Category | Must answer |
|----------|-------------|
| Problem | What it solves, who has this problem |
| Users | Primary users, technical level |
| Features | 3-5 MVP features (must-have vs nice-to-have) |
| Tech Stack | Preferences or constraints |
| Scale | Users at launch, data volume |
| Auth | Need accounts? What method? |
| Data | What's stored? PII or payments? |
| Integrations | External APIs or services |
| Non-goals | Explicitly out of scope |

## Interview Rules

- Ask 2-3 questions at a time — never all at once
- Follow up on vague answers
- Never assume — ask if unsure

## Confirmation Before Writing

After interview, present summary to user:

```
Here's what I captured — confirm before I write conversation.md:

Problem: [summary]
Users: [summary]
Features: [list]
Tech Stack: [summary]
Non-goals: [list]

Accurate? (yes / correct [X] / abort)
```

**Do NOT write `conversation.md` until user explicitly confirms.**

## Output — conversation.md

```markdown
# Project Brief

## What It Does
[2-3 sentences: problem, users, why it matters]

## Users
- Primary: [type + technical level]

## Core Features (MVP)
1. [Feature] — [acceptance criteria]

## Nice-to-Have
- [Feature]

## Tech Stack
- Backend / Frontend / Database / Auth / Infrastructure

## Constraints & Integrations
## Non-Goals
## Scale Expectations
```

## Done Signal

Tell user: "`conversation.md` written. Run `/cortexweave:weave-plan` to decompose into tasks."
