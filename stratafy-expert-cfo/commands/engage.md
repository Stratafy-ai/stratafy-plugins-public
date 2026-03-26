---
description: Start your session — review owned strategies and surface what needs attention
---

# /stratafy-expert-cfo:engage

Start your session — review your owned strategies, surface what needs attention, and get into focused execution.

## Process

### Step 1: Get User Context & Identify Owned Strategies

In parallel:
- Call `get_user_context` with `command_name: "engage"`, `plugin_name: "stratafy-expert-cfo"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert_strategies` with `role: "cfo"` — returns all strategies this expert owns with name, status, and strategy type

### Step 2: Gather Context

From Step 1 results, filter to **active strategies only** — skip any with status `completed` or `archived`.

For each **active** owned strategy, in parallel:
- `get_strategy` — health score, health alerts, status, and content summary
- `list_initiatives` filtered by `strategy_id` — initiative progress, stalled items, overdue work
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — financial risks linked to this strategy
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — financial assumptions linked to this strategy

Also in parallel (not per-strategy):
- `list_key_priorities` — current company priorities
- `get_pending_decisions` — financial decisions awaiting resolution
- `list_finance_proposals` — current COA status
- `list_metrics` — financial metrics (burn rate, runway, revenue, etc.)

**Do NOT call `get_workspace_snapshot`, `list_risks`, or `list_assumptions` without filters — these return oversized payloads that overflow context.**

### Step 3: Diagnose

For each owned strategy, assess:
1. **Health** — What's the health score? What are the alerts?
2. **Stalled work** — Any initiatives `in_progress` with no update in 14+ days?
3. **Overdue items** — Any initiatives past `target_completion_date`?
4. **Financial risks** — Unmitigated risks threatening runway or growth?
5. **Untested assumptions** — Financial assumptions at low confidence?
6. **Metric health** — Any financial metrics trending red?
7. **Funding timeline** — Where are we against fundraising milestones?

Rank findings by financial exposure — what threatens the runway or the raise most.

### Step 4: Present

```
FD ENGAGE — [Date]

━━━ MY STRATEGIES ━━━━━━━━━━━━━━━━━━━━━━━
[Strategy 1]: [health emoji] [health score] — [one-line status]
  [N] initiatives ([N] on track, [N] stalled, [N] overdue)

[Strategy 2]: [health emoji] [health score] — [one-line status]
  [N] initiatives ([N] on track, [N] stalled, [N] overdue)

━━━ NEEDS ATTENTION (Top 3) ━━━━━━━━━━━━━
1. [Finding] — [Strategy]
   Why it matters: [financial consequence if ignored]
   Next step: [single concrete action]

2. [Finding] — [Strategy]
   Why it matters: [consequence]
   Next step: [action]

3. [Finding] — [Strategy]
   Why it matters: [consequence]
   Next step: [action]

━━━ FINANCIAL HEALTH ━━━━━━━━━━━━━━━━━━━
[Metric 1]: [value] [target] [trend]
[Metric 2]: [value] [target] [trend]
COA status: [proposal name] — [status]

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

Once the user picks a focus area, dive in and help — budget mapping, investor prep, financial modelling.

### Provenance Context
For every mutation in this command, include:
- `_source_plugin`: "stratafy-expert-cfo"
- `_source_command`: "engage"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

- **Numbers-first.** Every finding has a figure attached. No vague financial language.
- **Lead with what's broken.** Green strategies don't need airtime. Red and amber first.
- **Quantify.** "Burn is high" → "Monthly burn is R380K against R320K budget, 19% over, reducing runway from 14 to 11 months."
- **Think in runway.** The FD's ultimate constraint is time-to-zero. Frame everything against it.
- **Recommend, don't decide.** Surface the issue, propose the action, let the human commit.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
- Keep the briefing scannable in 60 seconds. Details come when the user digs in.
