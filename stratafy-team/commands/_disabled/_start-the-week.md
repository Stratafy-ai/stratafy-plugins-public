---
description: "Monday briefing — metrics health, key priorities, and what matters this week"
---

# /stratafy-team:start-the-week

Monday briefing — metrics health, key priorities, what changed, and what matters for your role this week.

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "start-the-week"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Gather Context

Everything needed in parallel:
- `get_workspace_snapshot` — Company context, stage, foundation
- `list_metrics` — All metrics with current values, targets, thresholds
- `list_key_priorities` — Current company-wide priorities
- `list_strategies` — Active strategies with status
- `list_initiatives` — Active initiatives with progress
- `list_risks` — Open risks (look for new or escalated)
- `list_decisions` — Recent decisions (last 7 days)
- `list_insights` — Recent insights (last 7 days)

### Step 3: Build the Briefing

Adapt the entire briefing to the user's role (see role-adaptation skill).

```
WEEK BRIEFING — [Company Name]
Week of [date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 KEY PRIORITIES THIS WEEK
1. [Priority] — [one line on status/progress]
2. [Priority] — [one line on status/progress]
3. [Priority] — [one line on status/progress]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 METRICS SNAPSHOT
[Group by what matters to this role]

[Category relevant to role]:
  [Metric]:  [value]/[target]  🟢/🟡/🔴  [one line interpretation]
  [Metric]:  [value]/[target]  🟢/🟡/🔴  [one line interpretation]

[Second category]:
  [Metric]:  [value]/[target]  🟢/🟡/🔴  [one line interpretation]

Summary: X 🟢  Y 🟡  Z 🔴 across [N] key metrics

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔄 WHAT CHANGED LAST WEEK
[Only include if there were actual changes — don't fabricate]

Decisions made:
  - [Decision] — [impact on this role]

Strategy changes:
  - [What shifted and why]

New risks:
  - [Risk] — [relevance to this role]

Insights captured:
  - [Insight] — [what it means]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔮 WHAT MATTERS FOR YOU THIS WEEK
[Role-specific focus areas — this is the key section]

For [role]:
1. [Specific action/focus] — because [strategic reason]
2. [Specific action/focus] — because [strategic reason]
3. [Specific action/focus] — because [strategic reason]

⚡ One thing to watch: [the most important signal for this role]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 4: Offer to Go Deeper

After presenting the briefing:
- "Want to plan your tasks for the week?" → transition to `/lets-go` territory
- "Want to dig into any of these metrics?"
- "Want to explore the [red metric] further?"

## Role-Specific Briefing Focus

**Engineering:** Product metrics, technical health, sprint status, upcoming releases, technical decisions
**Sales:** Pipeline metrics, deal status, competitive moves, GTM strategy updates, quota progress
**Marketing:** Content/traffic metrics, campaign performance, brand metrics, audience growth, channel health
**Operations:** Delivery metrics, process health, team capacity, operational risks, efficiency metrics
**Finance:** Cash metrics, burn rate, revenue, budget variance, financial risks, runway
**Leadership:** OKR progress, initiative portfolio, cross-team blockers, strategic alignment, key decisions needed

## Rules

- Keep it scannable — this is a briefing, not a report. Under 2 minutes to read.
- Only surface what's relevant to the role. An engineer doesn't need the full financial dashboard.
- "What changed" should only include actual changes — don't pad with "nothing changed" for every category.
- The "what matters for you" section is the most important part. Spend the most thought there.
- If there are red metrics, surface them prominently regardless of role — everyone should know.
- When referencing strategies, use markdown links with `urls.detail` URLs.
