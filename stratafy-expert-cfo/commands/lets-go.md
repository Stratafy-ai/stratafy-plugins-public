---
description: "Analyse your strategies and dive into the highest-impact financial work right now"
---

# /stratafy-expert-cfo:lets-go

Analyse the FD's owned strategies, identify the highest-impact financial work, and dive straight into execution.

## When to Use

- Starting a work session with the FD
- "What should I work on next?"
- When you have time and want to make the biggest financial impact
- After an engage session surfaced issues — this is where you fix them

## Process

### Step 1: Get User Context & Identify Owned Strategies

In parallel:
- Call `get_user_context` with `command_name: "lets-go"`, `plugin_name: "stratafy-expert-cfo"`. If you know the workspace ID (e.g., from project instructions), also pass `workspace_id` to set the session workspace.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert` with `role: "fd"` — returns the FD expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step — returns all strategies this expert owns.

### Step 2: Diagnose Strategy Health

From Step 1 results, filter to **active strategies only** — skip any with status `completed` or `archived`.

For each **active** owned strategy, in parallel:
- `get_strategy` — health score, health alerts, status
- `list_initiatives` filtered by `strategy_id` — initiative progress, stalled items
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — open risks
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — untested assumptions

Also in parallel (not per-strategy):
- `list_finance_proposals` — COA status
- `list_metrics` — financial metrics health
- `get_pending_decisions` — decisions blocking progress

**Do NOT call `get_workspace_snapshot`, `list_risks`, or `list_assumptions` without filters — these return oversized payloads that overflow context.**

### Step 3: Prioritise Work Items

Score each potential work item by **financial impact × urgency**:

**High impact + urgent:**
- Unmitigated risks with financial exposure > 10% of runway
- Metrics trending red with no action plan
- Pending decisions blocking initiative progress
- COA with unmapped spend > 20%
- Assumptions at low confidence underpinning active initiatives

**High impact + not urgent:**
- Strategy with no financial backing (unfunded)
- Metrics that should exist but don't (measurement gaps)
- Quarterly review preparation (if within 2 weeks of quarter end)
- Investor prep work (if fundraising is active)

**Medium impact:**
- Stalled initiatives needing financial input
- Budget mapping gaps
- Pricing model review triggers

**Low impact (defer):**
- Minor mapping weight adjustments
- Cosmetic COA restructuring
- Metrics that are green and stable

### Step 4: Present the Action Plan

```
FD — LET'S GO
[Date]

Based on your [N] active strategies, here's where your time has the most financial impact right now:

━━━ PRIORITY 1 ━━━━━━━━━━━━━━━━━━━━━━━━━
[Action title]
Strategy: [strategy name] ([health])
Why now: [specific financial consequence of delay]
What we'll do: [concrete 2-3 step plan]
Time estimate: [~X minutes]

━━━ PRIORITY 2 ━━━━━━━━━━━━━━━━━━━━━━━━━
[Action title]
Strategy: [strategy name] ([health])
Why now: [reason]
What we'll do: [plan]
Time estimate: [~X minutes]

━━━ PRIORITY 3 ━━━━━━━━━━━━━━━━━━━━━━━━━
[Action title]
Strategy: [strategy name] ([health])
Why now: [reason]
What we'll do: [plan]
Time estimate: [~X minutes]

━━━ ALSO ON THE RADAR ━━━━━━━━━━━━━━━━━━
- [Item] — [why it can wait]
- [Item] — [why it can wait]

Pick a number, or tell me what else is on your mind.
```

### Step 5: Execute

When the user picks a priority (or brings their own focus):
1. Dive straight into execution — don't re-analyse
2. Use the relevant workflow: budget mapping, investor prep, risk mitigation, metric setup, etc.
3. After completing the work item, offer to move to the next priority
4. Track what was accomplished for the session

### Provenance Context

For every mutation in this command, include:
- `_source_plugin`: "stratafy-expert-cfo"
- `_source_command`: "lets-go"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Action over analysis.** This command exists to DO things, not report on things. Engage is the report. Lets-go is the work session.
2. **Rank by financial impact.** Not by strategy priority, not by ease — by what moves the financial needle most.
3. **Be specific.** "Fix budget mapping" → "Map the 8 unmapped expense accounts in the 5000 series to close the 23% structural gap."
4. **Time-box suggestions.** Every priority should have a rough time estimate so the user can plan their session.
5. **Numbers-first.** Every priority has a figure. "Risk is high" → "R120K/month unmitigated exposure on cloud cost escalation."
6. **Recommend the first one.** Don't just present options — say which one you'd start with and why.
7. **Execute, don't plan.** When the user picks a priority, start doing the work immediately.
8. When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
