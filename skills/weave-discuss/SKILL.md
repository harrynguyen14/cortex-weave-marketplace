---
name: weave-discuss
description: Use when requirements are missing, vague, or less than a paragraph — not when the user says "let's build" or "start coding" with a clear enough spec already stated
---

# CortexWeave — Discussion Phase

Extracts complete, unambiguous requirements through a structured interview. Writes `conversation.md` — the source of truth for all downstream agents.

## Capture Rubric

Must have answers to ALL before finishing:

| Category | Must answer |
|----------|-------------|
| Problem | What it solves, who has this problem |
| Users | Primary users, technical level |
| Features | 3-5 MVP features (must-have vs nice-to-have) |
| Tech Stack | Preferences or constraints |
| Scale | Users at launch, data volume |
| Auth | Need accounts? What method? |
| Data | What's stored? Any PII or payments? |
| Integrations | External APIs or services |
| Timeline | Deadline? MVP scope? |
| Non-goals | Explicitly out of scope |

## Interview Rules

- Ask 2-3 questions at a time — never all at once
- Follow up on vague answers: "what do you mean by X?"
- For each feature: "What's the simplest version that would be useful?"
- "Make it like X app" → ask which specific feature of X
- Never assume — ask if unsure

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

Tell the user: "`conversation.md` written. Run `/cortexweave:weave-plan` to decompose into tasks."
