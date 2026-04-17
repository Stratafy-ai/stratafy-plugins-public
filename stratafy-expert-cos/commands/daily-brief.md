---
description: Morning strategic briefing — what happened, what needs attention today
---

# /stratafy-cos:daily-brief

Morning strategic briefing — what happened yesterday, what needs attention today, and where focus should go. Designed to be the first thing a founder reads to start the day with full context.

## When to Use

- Start of each workday
- When returning after time away ("what did I miss?")
- Before any strategic conversation where you need full context

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "daily-brief"`.

### Step 2: Gather Data

In parallel:
- `generate_digest(period: 'daily', days_ago: 0)` — Yesterday's provenance digest (what changed and why)
- `list_strategies` — All strategies with health scores (health_score, health_status, health_alerts)
- `list_key_priorities` — Current company priorities
- `get_pending_decisions` — Decisions awaiting action
- `list_initiatives(status: 'in_progress')` — Active initiatives with progress
- `get_high_risk_items` — Elevated risks

**Note:** Use `list_strategies` not `get_workspace_snapshot` — the snapshot strips health scores. `list_strategies` returns `health_score`, `health_status`, and `health_alerts` for each strategy.

### Step 3: Check for Alerts

- Any strategies with health score changes (declining health = flag)
- Any initiatives past their target completion date
- Any risks that changed status yesterday (check provenance for risk mutations)
- Any new decisions created but not yet resolved

### Step 4: Generate the Brief

```
☀️ DAILY BRIEF — [Date]

━━━ YESTERDAY'S STRATEGIC ACTIVITY ━━━━━
[Provenance digest narrative — who changed what, why, patterns]
[X mutations | Y plugins active | Z% with reasoning]

━━━ NEEDS YOUR ATTENTION ━━━━━━━━━━━━━━
🔴 [Critical item] — [what it is, why it's urgent, recommended action]
⏳ [Pending decision] — [context, deadline, recommendation]
📊 [Overdue initiative] — [what's late, by how much, impact]

━━━ STRATEGIC HEALTH ━━━━━━━━━━━━━━━━━━
[Strategy 1]: 🟢/🟡/🔴 [score] — [one-line status]
[Strategy 2]: 🟢/🟡/🔴 [score] — [one-line status]
(Only show strategies with changes or alerts)

━━━ TODAY'S FOCUS ━━━━━━━━━━━━━━━━━━━━━
1. [Top priority — what and why]
2. [Second priority — what and why]
3. [Third priority — what and why]

━━━ BLIND SPOTS ━━━━━━━━━━━━━━━━━━━━━━━
[Strategies with no recent activity]
[Plugins not used in 7+ days]
[Key assumptions approaching validation deadline]
```

### Step 5: Persist the Brief

Render the brief body to the user first. Then persist it so it survives the chat and becomes a first-class workspace entity:

1. **Create a draft.** Call `create_brief` with:
   - `brief_type: "daily"`
   - `title`: "Daily Brief — [YYYY-MM-DD]"
   - `description`: the top line from "needs your attention"
   - `content`: the rendered markdown body
   - `source_plugin: "stratafy-expert-cos"`
   - `source_command: "daily-brief"`
   - Plus provenance.

2. **Release it.** Call `release_brief` with the brief `id` plus:
   - `content`: final body
   - `generation_qa`: empty array (daily-brief is unprompted).
   - `context_refs`: `{ strategy_ids, initiative_ids, decision_ids, risk_ids }` — IDs pulled in Step 2.
   - `context_snapshot`: the "Yesterday's strategic activity" narrative as the snapshot.
   - Same provenance.

3. **Show the persistent URL.** Tell the user: "Saved as brief `<brief_id>` — `https://stratafy.ai/ws/<workspace_id>/brief/<brief_id>`."

### Step 6: Offer Follow-ups

Based on what surfaced, offer:
- "Want me to dig into [specific issue]?"
- "Should I prepare a decision brief for [pending decision]?"
- "Want me to run an alignment check on [strategy with health change]?"

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-cos"
- `_source_command`: "daily-brief"
- `_change_reasoning`: brief explanation of why the change was made

## Rules

1. **The provenance digest is the headline.** It tells the story of what happened. Lead with it.
2. **Only surface what needs attention.** A healthy strategy with no changes doesn't need a line.
3. **Blind spots matter.** Strategies with zero provenance records are potentially drifting — flag them.
4. **Be opinionated about focus.** Don't list 10 priorities. Pick 3 based on urgency, strategic impact, and what the provenance data suggests momentum on.
5. **Keep it scannable.** The whole brief should take 2 minutes to read. Use the structure above, not paragraphs.
6. **Connect yesterday to today.** "Yesterday the CTO restructured 5 strategies. Today you should review the new Strategic Delivery strategy before the team operates against it."
7. When referencing strategies or initiatives, use markdown links with `urls.detail` URLs from tool responses.
