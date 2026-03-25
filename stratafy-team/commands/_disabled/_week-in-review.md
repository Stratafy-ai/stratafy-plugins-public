---
description: "Friday wrap-up — what the company accomplished against priorities"
---

# /stratafy-team:week-in-review

Friday wrap-up — what the company accomplished this week against priorities, metrics that moved, and what's coming next week.

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "week-in-review"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Gather Context

Everything needed in parallel:
- `get_workspace_snapshot` — Company context
- `list_metrics` — Current metric values and status
- `list_key_priorities` — Current priorities
- `list_initiatives` — Initiative status and progress
- `list_decisions` — Decisions made this week
- `list_insights` — Insights captured this week
- `list_risks` — Risk changes this week

### Step 3: Build the Review

Adapt to the user's role (see role-adaptation skill).

```
WEEK IN REVIEW — [Company Name]
Week ending [date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ ACCOMPLISHED THIS WEEK

Against key priorities:
  🎯 [Priority 1]: [What happened — progress, completion, or no movement]
  🎯 [Priority 2]: [What happened]
  🎯 [Priority 3]: [What happened]

Notable completions:
  - [Initiative/task completed] → supports [strategy]
  - [Initiative/task completed] → supports [strategy]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 METRICS THAT MOVED
[Only metrics that changed — don't list static ones]

  [Metric]: [old] → [new]  [direction ↑↓→]  [one line on what drove the change]
  [Metric]: [old] → [new]  [direction ↑↓→]  [one line]

Unchanged but critical:
  [Metric]: Still at [value] — [why this matters: e.g., "3rd week at zero"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🧠 DECISIONS & INSIGHTS

Decisions made:
  - [Decision] — [impact]

Insights captured:
  - [Insight] — [implication]

Risks:
  - [New/escalated risk] — [status]
  - [Risk resolved] ✅

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔮 LOOKING AHEAD

Next week's focus:
  1. [What matters most next week for this role]
  2. [Second focus area]
  3. [Third focus area]

Coming up:
  - [Upcoming deadline, launch, or milestone]
  - [Meeting or review that needs preparation]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💬 ONE LINE SUMMARY
[A single sentence capturing the week: "Good week for product, quiet week for pipeline, funding readiness still stalled."]
```

### Step 4: Offer Reflection

"Anything you want to capture before the weekend? A learning, an observation, something that's been nagging you?"

If they share something valuable, log it as an insight via `create_insight` with tags including `"week_in_review"`.

## Rules

- This is a celebration + reality check, not an audit. Lead with accomplishments.
- Only show metrics that moved or are critically stagnant. Don't dump the whole dashboard.
- "Looking ahead" should be forward-looking and motivating, not a todo list.
- The one-line summary is the most important line — write it like a headline.
- Keep the whole review readable in under 2 minutes.
- When referencing strategies, use markdown links with `urls.detail` URLs.
