---
name: session-debrief
description: Parse strategy session notes, meeting transcripts, or coaching debrief notes into structured Stratafy artifacts
---

# /stratafy:session-debrief

Parse strategy session notes, meeting transcripts, or coaching debrief notes into structured Stratafy artifacts. Turns a conversation into strategies, initiatives, decisions, risks, assumptions, and insights.

## When to Use

- After a coaching session with a client
- After a board meeting or strategy offsite
- After a leadership team planning session
- After any meeting where strategic decisions were discussed

## Input

Provide one of:
1. **Raw session notes** — bullet points, rough notes, stream of consciousness
2. **Meeting transcript** — from Fireflies, Otter, or any transcription tool
3. **Voice memo summary** — describe what was discussed
4. **A conversation** — just tell me what happened and I'll extract the strategic content

## Process

### Step 1: Confirm Workspace
I'll verify we're in the correct client/organisation workspace. If you're a coach with multiple clients, tell me which client this session was for.

### Step 2: Parse & Categorise
I'll read through your input and identify:

**Decisions made** → `create_decision` or `update_decision`
- Choices that were finalised with rationale
- Status set to `decided` with today's date

**New strategic directions** → `create_strategy` or `update_strategy`
- New focus areas or pivots discussed
- Changes to existing strategy descriptions or status

**Action items / initiatives** → `create_initiative` or `update_initiative`
- Concrete work commitments with owners and timelines
- Updates to existing initiative status or priority

**Objectives set or updated** → `create_objective` or `update_okr_score`
- New targets or key results
- Score updates on existing OKRs

**Risks identified** → `create_risk`
- Threats, concerns, or potential problems raised
- Rated by likelihood and impact

**Assumptions surfaced** → `create_assumption`
- Beliefs stated or implied that underpin strategic choices
- Especially valuable when implicit assumptions are made explicit

**Insights captured** → `create_insight`
- Realisations, learnings, or new understanding
- "Aha moments" from the session

**Signals detected** → `create_signal`
- Market information, competitive intelligence, or trends mentioned
- Customer feedback or industry shifts discussed

### Provenance Context
For every mutation in this debrief, include:
- `_source_plugin`: "stratafy-guardian"
- `_source_command`: "session-debrief"
- `_change_reasoning`: Brief explanation tied to what was discussed (e.g. "Board discussed pivot to enterprise — creating new strategic direction")

### Step 3: Review Before Creating
Before writing anything to the workspace, I'll present a summary of what I plan to create:

```
From your session notes, I'll create:
- 2 decisions (pricing model, market focus)
- 1 new initiative (Q2 hiring plan)
- 3 assumptions (market size, competitor response, hiring timeline)
- 1 risk (key person dependency)
- 2 insights (customer segment behaviour, pricing sensitivity)

Want me to proceed, or adjust anything?
```

### Step 4: Create Artifacts
Once confirmed, I'll create all artifacts in the workspace, properly linked to relevant strategies and initiatives.

### Step 5: Session Summary
I'll generate a clean summary of the session outcomes:
- What was decided
- What actions were committed
- What was learned
- What needs follow-up
- Preparation notes for the next session

## Tips for Better Debriefs

- **Capture quotes** — exact language from the client is more valuable than your interpretation
- **Note what wasn't said** — sometimes the most important insight is what was avoided
- **Flag disagreements** — if team members disagreed, capture both perspectives
- **Record energy levels** — what topics generated excitement vs. anxiety?
- **Include context** — "this was discussed after we reviewed the Q1 numbers" helps me categorise correctly

## Supercharged with Connectors

If you have **Fireflies** connected, I can pull the transcript directly.
If you have **Google Calendar** connected, I can identify the meeting and any attached notes.
If you have **Slack** connected, I can check for any pre-meeting discussion context.
