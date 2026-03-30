---
description: "Analyse your strategies and dive into the highest-impact people work right now"
---

# /stratafy-expert-chro:lets-go

Analyse the CHRO's owned strategies, identify the highest-impact people work, and dive straight into execution.

## When to Use

- Starting a work session with the CHRO
- "What should I work on next?"
- When you have time and want to make the biggest people impact
- After an engage session surfaced issues — this is where you fix them

## Process

### Step 1: Get User Context & Identify Owned Strategies

In parallel:
- Call `get_user_context` with `command_name: "lets-go"`, `plugin_name: "stratafy-expert-chro"`. If you know the workspace ID (e.g., from project instructions), also pass `workspace_id` to set the session workspace.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert` with `role: "chro"` — returns the CHRO expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step — returns all strategies this expert owns.

### Step 2: Diagnose Strategy Health

From Step 1 results, filter to **active strategies only** — skip any with status `completed` or `archived`.

For each **active** owned strategy, in parallel:
- `get_strategy` — health score, health alerts, status
- `list_initiatives` filtered by `strategy_id` — initiative progress, stalled items
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — open risks
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — untested assumptions

Also in parallel (not per-strategy):
- `list_teams` — team structure
- `list_team_members` — ownership data
- `get_pending_decisions` — decisions blocking progress

**Do NOT call `get_workspace_snapshot`, `list_risks`, or `list_assumptions` without filters — these return oversized payloads that overflow context.**

### Step 3: Prioritise Work Items

Score each potential work item by **people impact x urgency**:

**High impact + urgent:**
- Capacity gaps — strategies with no lead or critically under-resourced
- Ownership gaps — initiatives with no owner blocking execution
- Concentration risk — single points of failure on critical strategies
- Stalled hiring — roles needed but no brief or process started
- Culture drift — decisions visibly misaligned with stated values
- Pending people decisions blocking initiative progress

**High impact + not urgent:**
- Team structure misaligned to strategy (restructure planning)
- Onboarding gaps for recent or upcoming hires
- Succession planning for key person dependencies
- Retention risk assessment needed

**Medium impact:**
- Values refresh or principles review
- Team health check overdue
- Org design review for scaling

**Low impact (defer):**
- Minor team description updates
- Documentation improvements
- Non-blocking process refinements

### Step 4: Present the Action Plan

```
CHRO — LET'S GO
[Date]

Based on your [N] active strategies, here's where your time has the most people impact right now:

━━━ PRIORITY 1 ━━━━━━━━━━━━━━━━━━━━━━━━━
[Action title]
Strategy: [strategy name] ([health])
Why now: [specific people consequence of delay]
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
2. Use the relevant workflow: hiring brief, org review, retention scan, onboarding plan, etc.
3. After completing the work item, offer to move to the next priority
4. Track what was accomplished for the session

### Provenance Context

For every mutation in this command, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "lets-go"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Action over analysis.** This command exists to DO things, not report on things. Engage is the report. Lets-go is the work session.
2. **Rank by people impact.** Not by strategy priority, not by ease — by what moves the people needle most.
3. **Be specific.** "Fix team structure" → "Assign a lead to the GTM strategy — it has 4 active initiatives and no owner."
4. **Time-box suggestions.** Every priority should have a rough time estimate so the user can plan their session.
5. **People-first.** Every priority connects to capacity, culture, ownership, or retention. No abstract org theory.
6. **Recommend the first one.** Don't just present options — say which one you'd start with and why.
7. **Execute, don't plan.** When the user picks a priority, start doing the work immediately.
8. When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
