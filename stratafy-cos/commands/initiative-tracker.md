# /stratafy-cos:initiative-tracker

Track and assess all active initiatives across the organisation. Surface what's on track, what's slipping, and what needs intervention — before it becomes a crisis.

## When to Use

- Weekly initiative review
- Preparing for a portfolio review meeting
- When something feels "stuck" but you can't pinpoint what
- Before resourcing or prioritisation decisions

## Input

Provide one of:
- **Nothing** — Review all active initiatives
- **A strategy area** — "track Product initiatives" or "show me GTM execution"
- **A status filter** — "show me what's at risk" or "what's overdue?"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "initiative-tracker"`.

### Step 2: Gather Data

In parallel:
- `list_initiatives` — All initiatives with status, progress, dates
- `list_strategies` — To map initiatives to their strategic homes
- `list_key_priorities` — To flag which initiatives serve top priorities
- `list_metrics` — To check if initiative metrics are moving

### Step 3: Enrich with Execution Data

For each initiative:
- `get_initiative` — Full detail including objectives, linked metrics
- If the initiative has a `[linear:PROJECT_ID]` tag, pull project data from Linear:
  - Issue counts by status (done, in-progress, todo, blocked)
  - Completion percentage
  - Recently blocked items

### Step 4: Assess Each Initiative

Rate each initiative on:
1. **Progress** — Is execution on track vs. timeline?
2. **Velocity** — Is the team making steady progress or stalling?
3. **Alignment** — Does the initiative still serve its parent strategy? Has the strategy shifted?
4. **Resourcing** — Is the initiative properly resourced? Signs of stretching?
5. **Impact** — Are the metrics this initiative should move actually moving?

### Step 5: Generate the Portfolio View

```
📊 INITIATIVE PORTFOLIO — [Date]
Active: [N] | On Track: [N] | At Risk: [N] | Stalled: [N]

━━━ NEEDS ATTENTION ━━━━━━━━━━━━━━━━━━━━

🔴 [Initiative Name]
   Strategy: [Parent strategy]
   Status: [current status] | Due: [date]
   Issue: [What's wrong — specific, not vague]
   Impact: [What happens if this isn't resolved]
   Recommendation: [Specific action]

🟡 [Initiative Name]
   Strategy: [Parent strategy]
   Status: [current status] | Due: [date]
   Watch: [What's trending — why it might become red]
   Mitigation: [What could prevent escalation]

━━━ ON TRACK ━━━━━━━━━━━━━━━━━━━━━━━━━━━

🟢 [Initiative Name] — [Strategy] — [progress %] — [one-line status]
🟢 [Initiative Name] — [Strategy] — [progress %] — [one-line status]

━━━ PORTFOLIO INSIGHTS ━━━━━━━━━━━━━━━━━

📌 CONCENTRATION RISK
[Are too many initiatives under one strategy? Is one team carrying too much?]

📌 RESOURCE CONFLICTS
[Are any teams or people assigned to competing initiatives?]

📌 STRATEGIC GAPS
[Are any key priorities or strategies without active initiatives?]

📌 VELOCITY TRENDS
[Which initiatives are accelerating? Which are decelerating? Why?]
```

### Step 6: Offer Next Steps

Based on the assessment, suggest:
- Which initiatives need a leadership check-in
- Which might benefit from scope adjustment
- Whether any should be paused or merged
- If new initiatives are needed to fill strategic gaps

## Rules

- **Red items first.** Always. Leadership attention is scarce — direct it to fires.
- **Be specific about problems.** "Initiative is behind" is useless. "Initiative is 3 weeks behind because the API integration blocker hasn't been resolved — the engineering team is waiting on a third-party response" is actionable.
- **Connect everything to strategy.** An initiative's health matters because of which strategy it serves.
- **Flag strategic gaps.** The absence of an initiative is sometimes more important than the health of existing ones.
- **Don't just track — interpret.** "12 of 20 issues are done" is data. "The team shipped 12 of 20 issues but the remaining 8 are the complex ones — expect the last 40% to take as long as the first 60%" is insight.
- When referencing initiatives or strategies, use markdown links with the `urls.detail` URL from tool responses.
