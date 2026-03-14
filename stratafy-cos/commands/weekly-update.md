# /stratafy-cos:weekly-update

Generate a weekly leadership update — the rhythm document that keeps the executive team aligned. Designed to replace the "going around the table" standup with something more structured and insightful.

## When to Use

- Every Monday morning (or Sunday evening for prep)
- As the standing weekly leadership communication
- When the CEO/founder needs a pulse on the business

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "weekly-update"`.

### Step 2: Gather Everything

In parallel:
- `get_workspace_snapshot` — Company context
- `list_strategies` — Strategic health
- `list_key_priorities` — Current focus areas
- `list_metrics` — All metrics with trends
- `list_initiatives` — Initiative portfolio
- `list_risks` — Active risks
- `get_pending_decisions` — Open decisions
- `list_signals` — Recent signals
- `list_decisions` — Recent decisions
- `list_assumptions` — Assumption health

### Step 3: Assess the Week

**What shipped / was completed:**
- Initiatives that hit milestones
- Decisions that were made
- Metrics that improved

**What slipped / went wrong:**
- Initiatives that missed deadlines
- Metrics that moved in the wrong direction
- Risks that escalated

**What changed:**
- New signals or market developments
- Strategy adjustments
- Priority shifts
- Team changes

### Step 4: Generate the Update

```
📊 WEEKLY UPDATE — Week of [Date]

━━━ THIS WEEK IN ONE LINE ━━━━━━━━━━━━
[The single most important takeaway from the week]

━━━ SCORECARD ━━━━━━━━━━━━━━━━━━━━━━━━

Key Priorities:
  [Priority 1]: 🟢/🟡/🔴 — [one-line status]
  [Priority 2]: 🟢/🟡/🔴 — [one-line status]
  [Priority 3]: 🟢/🟡/🔴 — [one-line status]

Key Metrics:
  [Metric 1]: [value] ([change from last week]) 🟢/🟡/🔴
  [Metric 2]: [value] ([change from last week]) 🟢/🟡/🔴
  [Metric 3]: [value] ([change from last week]) 🟢/🟡/🔴

━━━ WINS 🎉 ━━━━━━━━━━━━━━━━━━━━━━━━━
- [Win 1] — [which strategy/initiative it serves]
- [Win 2] — [which strategy/initiative it serves]

━━━ CONCERNS ⚠️ ━━━━━━━━━━━━━━━━━━━━━━
- [Concern 1] — [impact + what we're doing about it]
- [Concern 2] — [impact + what we're doing about it]

━━━ DECISIONS MADE ✅ ━━━━━━━━━━━━━━━━━
- [Decision] — [outcome and follow-ups]

━━━ DECISIONS NEEDED ⏳ ━━━━━━━━━━━━━━━
- [Decision needed] — [deadline + recommendation]

━━━ INITIATIVE HEALTH ━━━━━━━━━━━━━━━━━

🟢 On Track ([N]):
  [Initiative list — one line each]

🟡 Watch ([N]):
  [Initiative] — [why it's yellow]

🔴 At Risk ([N]):
  [Initiative] — [what's wrong + recommended action]

━━━ SIGNALS & INTELLIGENCE ━━━━━━━━━━━
📡 [Signal] — [relevance to our strategy]

━━━ NEXT WEEK FOCUS ━━━━━━━━━━━━━━━━━━
1. [Top priority for the coming week]
2. [Second priority]
3. [Third priority]

━━━ CALENDAR AHEAD ━━━━━━━━━━━━━━━━━━━
[Key meetings, deadlines, milestones in the next 7-14 days]
```

### Step 5: Highlight Trends

Don't just report this week in isolation. Show:
- **Week-over-week metric changes** — are things getting better or worse?
- **Initiative velocity** — are things speeding up or slowing down?
- **Risk trajectory** — are risks being mitigated or accumulating?
- **Decision throughput** — are decisions being made or piling up?

## Rules

- **Same format every week.** Consistency builds rhythm. People know where to look for what.
- **Honest, not optimistic.** A weekly update that's always positive is useless. When things are hard, say so. Leadership needs truth, not comfort.
- **Highlight what changed, not just what is.** "Revenue is R100K" is less useful than "Revenue dropped from R112K to R100K — the 3 enterprise deals we expected to close slipped to next month."
- **Keep it under 2 pages.** If it's longer, you're reporting, not updating. Summarise ruthlessly.
- **Always end with next week's focus.** This creates a natural commitment device.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
