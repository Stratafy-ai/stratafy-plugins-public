---
description: Quick health check — red/amber/green per owned strategy
---

# /stratafy-expert-cto:status

Quick health check — red/amber/green per owned strategy, with attention items and upcoming deadlines surfaced at the top.

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "status"`, `plugin_name: "stratafy-cto"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Get Owned Strategies

Call `get_expert_strategies` with `_source_plugin: "stratafy-cto"` with the CTO expert ID.

If that fails, fall back to `list_strategies` with `_source_plugin: "stratafy-cto"` and filter for technology/product/infrastructure strategies.

### Step 3: Get Health & Initiative Data

From Step 2 results, filter to **active strategies only** — skip any with status `completed` or `archived`.

For each **active** owned strategy, in parallel:
- `get_strategy` with `_source_plugin: "stratafy-cto"` — Health score, alerts, and status
- `list_initiatives` filtered by `strategy_id` with `_source_plugin: "stratafy-cto"` — Status, target_completion_date, completion_percentage

### Step 4: Identify What Needs Attention

From the gathered data, extract:

**Attention items** (show these first):
- Strategies with health status amber or red
- Health alerts with severity "red"
- Initiatives that are `in_progress` with no update in 14+ days (stalled)
- Initiatives past their `target_completion_date` (overdue)

**Upcoming deadlines** (next 14 days):
- Initiatives with `target_completion_date` within the next 14 days
- Sort by date, nearest first

### Step 5: Present

```
CTO STATUS — [Date]

━━━ NEEDS ATTENTION ━━━━━━━━━━━━━━━━━━━━
🔴 [Strategy/initiative] — [one-line reason]
🟡 [Strategy/initiative] — [one-line reason]
⚠️ [Initiative] — stalled [N] days, no update since [date]

━━━ UPCOMING DEADLINES ━━━━━━━━━━━━━━━━━
📅 [date] — [Initiative] ([completion]%) → [Strategy]
📅 [date] — [Initiative] ([completion]%) → [Strategy]

━━━ STRATEGY HEALTH ━━━━━━━━━━━━━━━━━━━━
[Strategy 1]: 🟢 [score] — [N] initiatives on track
[Strategy 2]: 🟡 [score] — [one-line issue]
[Strategy 3]: 🔴 [score] — [one-line issue]
```

If there are no attention items or upcoming deadlines, say so — that's a good sign.

Run `/stratafy-expert-cto:engage` if you want the full briefing with recommendations.

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "status"
- `_change_reasoning`: brief explanation

## Rules

- **Fast.** This should take seconds. Lead with what needs attention, not a full report.
- **Attention first.** Red and amber items at the top. Green strategies get one line, no detail.
- **Deadlines are concrete.** Show the actual date, not "soon" or "upcoming".
- **No recommendations.** That's what `/engage` is for. Status just shows the state.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
