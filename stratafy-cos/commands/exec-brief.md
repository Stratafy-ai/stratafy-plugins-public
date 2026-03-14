# /stratafy-cos:exec-brief

Generate a concise executive briefing — the one-page view of everything leadership needs to know right now. Designed to be scanned in 2 minutes and acted on immediately.

## When to Use

- Before a leadership meeting or standup
- Start of the week for the CEO/founder
- Before a board call (as a personal prep tool)
- When someone asks "what do I need to know?"

## Input

Provide one of:
- **Nothing** — Generate a full company brief
- **A focus area** — "brief me on GTM" or "brief me on product"
- **A time window** — "what changed since Friday?" or "brief me on the last 48 hours"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "exec-brief"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context, mission, vision, foundation
- `list_strategies` — All active strategies with status
- `list_key_priorities` — Current company priorities
- `list_metrics` — Key metrics with status and trends
- `list_initiatives` — Active initiatives with progress
- `list_risks` — Active risks with severity
- `get_pending_decisions` — Decisions awaiting resolution
- `list_signals` — Recent signals that may need attention

### Step 3: Check Execution Health

For each active strategy:
- `get_strategy` — Get detail including linked initiatives and objectives
- Check initiative progress — any stalled, overdue, or at risk?
- Check OKR scores — any objectives trending red?

### Step 4: Pull Recent Activity

If Linear is connected:
- `list_issues` — Recently completed, newly blocked, or overdue across all projects
- Look for patterns: multiple teams blocked on the same thing, velocity drops, scope creep

### Step 5: Generate the Brief

```
📋 EXECUTIVE BRIEF — [Date]
Last updated: [timestamp]

━━━ HEADLINE ━━━━━━━━━━━━━━━━━━━━━━━━━━━
[One sentence: the single most important thing to know right now]

━━━ STRATEGIC HEALTH ━━━━━━━━━━━━━━━━━━━
[Strategy 1]: 🟢/🟡/🔴 — [one-line status]
[Strategy 2]: 🟢/🟡/🔴 — [one-line status]
[Strategy 3]: 🟢/🟡/🔴 — [one-line status]

━━━ KEY METRICS ━━━━━━━━━━━━━━━━━━━━━━━━
[Metric 1]: [value] [target] 🟢/🟡/🔴 [trend ↑↓→]
[Metric 2]: [value] [target] 🟢/🟡/🔴 [trend ↑↓→]
[Metric 3]: [value] [target] 🟢/🟡/🔴 [trend ↑↓→]

━━━ NEEDS YOUR ATTENTION ━━━━━━━━━━━━━━
🔴 [Issue 1] — [what happened, what's needed, recommended action]
🟡 [Issue 2] — [what's trending, when it becomes critical]
⏳ [Decision pending] — [what's waiting, deadline, recommendation]

━━━ WINS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ [Win 1] — [what was achieved, which strategy it serves]
✅ [Win 2] — [what was achieved, which strategy it serves]

━━━ UPCOMING ━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 [This week]: [key milestones, deadlines, events]
📅 [Next week]: [what's coming]

━━━ SIGNALS ━━━━━━━━━━━━━━━━━━━━━━━━━━━
📡 [Signal 1] — [what it means for us]
📡 [Signal 2] — [what it means for us]
```

## Rules

- **Brevity is everything.** This is an executive brief, not a report. Every word earns its place.
- **Lead with what needs attention.** Good news is nice but doesn't require action. Red items first.
- **Always include a headline.** Force yourself to pick the single most important thing. If you can't, that's a signal in itself.
- **Quantify everything.** "Revenue is down" → "Revenue dropped 12% MoM from R120K to R105K."
- **Connect to strategy.** Every issue and win links to a strategy. If it doesn't, question whether it belongs in an exec brief.
- **Be opinionated.** Don't just report — interpret. "Revenue dropped 12%" is reporting. "Revenue dropped 12% — likely driven by the delayed product launch, which pushed 3 enterprise deals into next quarter" is chief-of-staff thinking.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
