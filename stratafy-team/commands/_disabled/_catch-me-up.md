# /stratafy-team:catch-me-up

What changed while you were away — strategy shifts, decisions made, metrics that moved, and what you need to know to get back up to speed.

## Input

Provide one of:
- **Nothing** — I'll cover the last 7 days
- **A timeframe** — "catch me up on the last 2 weeks" or "what happened since Monday?"
- **A specific area** — "catch me up on the product strategy" or "what happened with the pipeline?"

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "catch-me-up"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Gather Context

Everything needed in parallel:
- `get_workspace_snapshot` — Current state of the workspace
- `list_metrics` — Current metric values and trends
- `list_key_priorities` — Current priorities (may have changed)
- `list_strategies` — Strategy status (look for status changes)
- `list_initiatives` — Initiative progress and status changes
- `list_decisions` — Decisions made in the period
- `list_insights` — Insights captured in the period
- `list_risks` — New, escalated, or resolved risks
- `list_objectives` — OKR progress changes

For metric trends:
- `get_metric_trend` — For any metric that appears to have moved significantly

### Step 3: Identify What Actually Changed

Filter ruthlessly. Only surface things that:
- **Changed state** — Strategy status shifted, initiative completed, risk escalated
- **Moved significantly** — Metric crossed a threshold or changed direction
- **Require awareness** — Decisions that affect this person's role
- **Are new** — Risks, insights, or priorities that didn't exist before

Don't list things that stayed the same.

### Step 4: Present the Catch-Up

Adapt to the user's role (see role-adaptation skill).

```
CATCH-UP — [Company Name]
Covering: [date range]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚡ THE HEADLINE
[One paragraph: what's the single most important thing to know?
This should be the thing that, if they only read one line, would
get them 80% caught up.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 THINGS THAT NEED YOUR ATTENTION
[Only if there are items requiring action from this person]
  - [Item] — [what they need to do and by when]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 METRICS THAT MOVED

  [Metric]: [before] → [now]  [↑↓]  🟢/🟡/🔴
    [What drove the change]

  [Metric]: [before] → [now]  [↑↓]  🟢/🟡/🔴
    [What drove the change]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 DECISIONS MADE
[Only decisions relevant to this role]
  - [Decision] — [impact on their work]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔄 STRATEGY & INITIATIVE CHANGES
[Only actual status changes]
  - [Initiative/strategy] changed from [old status] to [new status]
  - [New initiative] launched under [strategy]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ NEW RISKS
  - [Risk] — [severity and relevance to this role]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 WHERE THINGS STAND NOW
[Current key priorities and how they've evolved]
  1. [Priority] — [current status]
  2. [Priority] — [current status]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ready to dive in? I can pull up your tasks with /lets-go or dig deeper into anything above.
```

## Rules

- The headline is everything. Get the essence into one paragraph.
- "Things that need your attention" comes before nice-to-know. Urgency first.
- Don't list things that didn't change. If nothing changed in a category, skip it entirely.
- Filter by role relevance — an engineer doesn't need to know about every sales decision unless it affects product.
- If the person was away for more than 2 weeks, group changes by week for easier digestion.
- Offer to transition into action: pull tasks, dig into a metric, explore a decision.
