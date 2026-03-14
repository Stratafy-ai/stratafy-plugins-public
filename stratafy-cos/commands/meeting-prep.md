# /stratafy-cos:meeting-prep

Prepare a comprehensive briefing pack for any leadership meeting — board meetings, exec standups, all-hands, investor updates, or 1:1s. Ensures the right context is gathered and the right talking points are prepared.

## When to Use

- Before a board meeting
- Before a weekly leadership standup
- Before an investor call or update
- Before a cross-functional review
- Before a 1:1 with a direct report about strategic topics

## Input

Provide:
- **Meeting type** — "board meeting", "exec standup", "investor call", "all-hands", "1:1 with [name/role]"
- **Optional: specific topics** — "focus on Q1 results" or "we need to discuss the product pivot"
- **Optional: audience context** — "the board is concerned about burn rate" or "this investor hasn't seen an update in 3 months"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "meeting-prep"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Full company context
- `list_strategies` — Active strategies
- `list_key_priorities` — Current priorities
- `list_metrics` — Metrics with trends
- `list_initiatives` — Initiative health
- `get_pending_decisions` — Open decisions
- `list_risks` — Active risks
- `get_high_risk_items` — Highest severity items
- `get_latest_review` — Most recent strategic review

If meeting-specific:
- For **board meetings**: Focus on strategy health, financial metrics, risk exposure, key decisions
- For **investor calls**: Focus on traction metrics, growth, burn rate, milestones, ask
- For **all-hands**: Focus on wins, progress, priorities, culture
- For **1:1s**: Focus on the person's domain, their initiatives, their metrics

### Step 3: Check Calendar Context

If Google Calendar is connected:
- Check the meeting details — who's attending, agenda if available
- Look at recent meetings with the same audience for continuity

### Step 4: Generate the Briefing Pack

#### For Board Meetings

```
📋 BOARD MEETING PREP — [Date]

━━━ PRE-READ SUMMARY ━━━━━━━━━━━━━━━━━━
[3-4 sentence executive summary of company status]

━━━ STRATEGIC UPDATE ━━━━━━━━━━━━━━━━━━
[Strategy health overview — traffic light for each pillar]
[Key progress since last board meeting]
[Strategic shifts or pivots to discuss]

━━━ FINANCIAL SNAPSHOT ━━━━━━━━━━━━━━━━━
[Key financial metrics: revenue, burn, runway, unit economics]
[Variance from plan with explanation]
[Financial decisions needed]

━━━ KEY METRICS ━━━━━━━━━━━━━━━━━━━━━━━
[Top 5-7 metrics with trends and context]

━━━ RISKS & MITIGATION ━━━━━━━━━━━━━━━━
[Top 3 risks with status and mitigation]
[Any new risks since last meeting]

━━━ DECISIONS NEEDED ━━━━━━━━━━━━━━━━━━
[Decisions for board input/approval]
[Context and recommendation for each]

━━━ TALKING POINTS ━━━━━━━━━━━━━━━━━━━━
[Things to proactively mention]
[Anticipated questions and prepared answers]

━━━ WATCH LIST ━━━━━━━━━━━━━━━━━━━━━━━━
[Things that might come up — be prepared]
```

#### For Exec Standups

```
📋 EXEC STANDUP PREP — [Date]

SINCE LAST STANDUP:
- [Key developments — 3-5 bullets]
- [Decisions made and their outcomes]
- [New blockers or escalations]

THIS WEEK'S FOCUS:
- [Top priorities — what must happen]
- [Key milestones or deadlines]

NEEDS DISCUSSION:
- [Topics requiring group input]
- [Cross-functional dependencies to resolve]

DECISIONS NEEDED:
- [Decision + recommendation + urgency]
```

#### For Investor Calls

```
📋 INVESTOR UPDATE PREP — [Date]

NARRATIVE:
[The story arc: where we were → what we did → where we are → where we're going]

TRACTION:
[Key growth metrics with trends]
[Milestone progress]

FINANCIALS:
[Revenue, burn, runway]
[Capital efficiency metrics]

ASK / UPDATE:
[What you need from this investor, or key updates they care about]

ANTICIPATED QUESTIONS:
[Q + prepared answer for each]
```

### Step 5: Prepare Anticipatory Questions

Based on the data, generate:
- 5-10 questions the audience is likely to ask
- A prepared response for each, grounded in actual data
- Flag any questions where the honest answer is uncomfortable — prepare for those especially

## Rules

- **Tailor to the audience.** A board meeting and an exec standup need completely different depths. Don't over-prepare a standup or under-prepare a board meeting.
- **Prepare for the hard questions.** The most valuable prep isn't the slide deck — it's knowing what awkward question is coming and having a clear, honest response ready.
- **Include what NOT to say.** Sometimes the prep is "don't bring up X unless asked — here's why and here's the response if it comes up."
- **Lead with the narrative.** Numbers without a story confuse. A story without numbers is hand-waving. Always pair them.
- **Check continuity.** What was discussed last time? What was promised? What's the update on those promises? Dropping threads erodes trust.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
