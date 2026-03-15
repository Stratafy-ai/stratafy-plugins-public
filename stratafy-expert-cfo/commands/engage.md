# /stratafy-fd:engage

Start your session — review your owned strategies, surface what needs attention, and get into focused execution.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "engage"`.

### Step 2: Identify Owned Strategies

Call `get_expert_strategies` with the FD expert ID to get all strategies this expert owns.

If that fails, fall back to `list_strategies` and filter for funding/finance/growth strategies.

### Step 3: Gather Context

For each owned strategy, in parallel:
- `get_strategy` — Get health score, health alerts, status, and content summary
- `list_initiatives` filtered by `strategy_id` — Get initiative progress, stalled items, overdue work
- `list_risks` — Financial risks
- `list_assumptions` — Financial assumptions that need validation
- `list_metrics` — Financial metrics (burn rate, runway, revenue, etc.)

Also in parallel:
- `get_workspace_snapshot` — Company context and current priorities
- `list_key_priorities` — What the company is focused on right now
- `get_pending_decisions` — Financial decisions awaiting resolution
- `list_finance_proposals` — Current COA status

### Step 4: Diagnose

For each owned strategy, assess:
1. **Health** — What's the health score? What are the alerts?
2. **Stalled work** — Any initiatives `in_progress` with no update in 14+ days?
3. **Overdue items** — Any initiatives past `target_completion_date`?
4. **Financial risks** — Unmitigated risks threatening runway or growth?
5. **Untested assumptions** — Financial assumptions at low confidence?
6. **Metric health** — Any financial metrics trending red?
7. **Funding timeline** — Where are we against fundraising milestones?

Rank findings by financial exposure — what threatens the runway or the raise most.

### Step 5: Present

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

### Step 6: Execute Together

Once the user picks a focus area, dive in and help — budget mapping, investor prep, financial modelling.

## Rules

- **Numbers-first.** Every finding has a figure attached. No vague financial language.
- **Lead with what's broken.** Green strategies don't need airtime. Red and amber first.
- **Quantify.** "Burn is high" → "Monthly burn is R380K against R320K budget, 19% over, reducing runway from 14 to 11 months."
- **Think in runway.** The FD's ultimate constraint is time-to-zero. Frame everything against it.
- **Recommend, don't decide.** Surface the issue, propose the action, let the human commit.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
- Keep the briefing scannable in 60 seconds. Details come when the user digs in.
