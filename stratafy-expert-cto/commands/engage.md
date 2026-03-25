---
description: Start your session — review owned strategies and surface what needs attention
---

# /stratafy-cto:engage

Start your session — review your owned strategies, surface what needs attention, and get into focused execution.

## Process

### Step 1: Get User Context & Identify Owned Strategies

In parallel:
- Call `get_user_context` with `command_name: "engage"`, `plugin_name: "stratafy-cto"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert_strategies` with `role: "cto"` — returns all strategies this expert owns with name, status, and strategy type

### Step 2: Gather Execution Detail

From Step 1 results, filter to **active strategies only** — skip any with status `completed` or `archived`.

For each **active** owned strategy, in parallel:
- `get_strategy` with the strategy ID — health score, alerts, status, content summary
- `list_initiatives` filtered by `strategy_id` — initiative progress, stalled items, overdue work
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — risks linked to this strategy
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — assumptions linked to this strategy

Also in parallel (not per-strategy):
- `list_key_priorities` — current company priorities
- `get_pending_decisions` — decisions awaiting resolution (scan results for tech-relevant ones)

**Do NOT call `get_workspace_snapshot` or `list_risks`/`list_assumptions` without filters — these return oversized payloads that overflow context.**

### Step 3: Diagnose

For each owned strategy, assess:
1. **Health** — What's the health score? What are the alerts?
2. **Stalled work** — Any initiatives `in_progress` with no update in 14+ days?
3. **Overdue items** — Any initiatives past `target_completion_date`?
4. **Unmitigated risks** — Any high-severity risks without mitigation?
5. **Untested assumptions** — Any critical assumptions still at low confidence?
6. **Missing coverage** — Strategies with no initiatives, no objectives, or no metrics?

Rank findings by strategic risk — what threatens execution most.

### Step 4: Present

```
CTO ENGAGE — [Date]

━━━ MY STRATEGIES ━━━━━━━━━━━━━━━━━━━━━━━
[Strategy 1]: [health emoji] [health score] — [one-line status]
  [N] initiatives ([N] on track, [N] stalled, [N] overdue)

[Strategy 2]: [health emoji] [health score] — [one-line status]
  [N] initiatives ([N] on track, [N] stalled, [N] overdue)

━━━ NEEDS ATTENTION (Top 3) ━━━━━━━━━━━━━
1. [Finding] — [Strategy]
   Why it matters: [strategic consequence if ignored]
   Next step: [single concrete action]

2. [Finding] — [Strategy]
   Why it matters: [strategic consequence]
   Next step: [action]

3. [Finding] — [Strategy]
   Why it matters: [strategic consequence]
   Next step: [action]

━━━ RISKS & ASSUMPTIONS ━━━━━━━━━━━━━━━━
🔴 [Risk] — [strategy] — [mitigation status]
🟡 [Assumption] — confidence [X]% — [needs validation?]

━━━ PENDING DECISIONS ━━━━━━━━━━━━━━━━━━
⏳ [Decision] — [what's blocking, recommendation]

━━━ READY TO WORK ON ━━━━━━━━━━━━━━━━━━━
Based on the above, here's where your time has the most impact today:
1. [Action] — [why now]
2. [Action] — [why now]
```

### Step 5: Execute Together

Once the user picks a focus area, dive in and help them do the work. Don't just brief — execute.

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "engage"
- `_change_reasoning`: brief explanation

## Rules

- **Be direct.** This is a CTO briefing, not a report. Architecture-first, no fluff.
- **Lead with what's broken.** Green strategies don't need airtime. Red and amber first.
- **Quantify.** "Initiative stalled" → "Initiative stalled 18 days, blocking Pulse v2 launch."
- **Think in dependencies.** Surface cross-strategy blockers — if Product Architecture is stuck, what does that mean for dependent strategies?
- **Recommend, don't decide.** Surface the issue, propose the action, let the human commit.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
- Keep the briefing scannable in 60 seconds. Details come when the user digs in.
