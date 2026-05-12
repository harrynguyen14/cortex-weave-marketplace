---
name: weave-discuss
description: Use when requirements are missing, vague, or less than a paragraph — not when the user says "let's build" or "start coding" with a clear enough spec already stated
---

# CortexWeave — Discussion Phase

Extracts complete requirements through an adaptive interview. Writes `conversation.md`.

## Step 1 — Parse What Already Exists

Before asking anything, extract from what the user has already said:

- Project type (web app, CLI, API, library, mobile, etc.)
- Tech stack preferences or constraints
- Features or goals already mentioned
- Users or audience
- Any explicit non-goals or constraints

Mark each rubric category as **known**, **partial**, or **missing** based on this parse.

## Step 2 — Adaptive Interview

Ask only about **missing** or **partial** categories. Skip what's already known.

**Grouping rules (determines how many questions per turn):**
- Ask related questions together in one turn (e.g., Auth + Data often go together; Scale + Integrations often go together)
- Ask unrelated questions separately
- If user gives a vague answer, follow up immediately before moving on
- If user gives a detailed answer that covers multiple categories, acknowledge and skip those

**Never ask about a category the user already answered.**

### Rubric Categories

| Category | Must answer | Skip if |
|----------|-------------|---------|
| Problem | What it solves, who has this problem | User already stated clear problem |
| Users | Primary users, technical level | User already described audience |
| Features | MVP features (must-have vs nice-to-have) | User already listed features |
| Tech Stack | Preferences or constraints | User already mentioned stack |
| Auth | Need accounts? What method? | Project type doesn't require auth (e.g., CLI tool, library) |
| Data | What's stored? PII or payments? | No persistence needed |
| Scale | Users at launch, data volume | MVP/prototype context makes this irrelevant |
| Integrations | External APIs or services | No external dependencies implied |
| Non-goals | Explicitly out of scope | Scope is already narrow and clear |

## Step 3 — Technical Advisory (On Demand)

When the user asks a specific technical question during the interview (e.g., "which database should I use?", "is X a good choice for this?", "what's the difference between A and B?"):

1. **Pause the interview**
2. **Search the web** for up-to-date information relevant to their stack and use case
3. **Respond with a grounded recommendation** — cite sources, include trade-offs, tailor to their context
4. **Resume the interview** from where you left off

Trigger phrases: "should I use", "which is better", "recommend", "what do you think about", "is X good for", "how does X compare"

## Step 4 — Confirmation Before Writing

After all missing categories are covered, present a summary:

```
Here's what I captured — confirm before I write conversation.md:

Problem: [summary]
Users: [summary]
Features: [list]
Tech Stack: [summary]
Non-goals: [list]
[other relevant categories]

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
