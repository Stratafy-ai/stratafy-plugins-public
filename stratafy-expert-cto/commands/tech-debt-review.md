---
description: Assess technical debt threatening strategic execution
---

# /stratafy-cto:tech-debt-review

Assess technical debt across the strategy portfolio. Surfaces where shortcuts, deferred work, and accumulated complexity threaten strategic execution — not just code quality.

## When to Use

- Before quarterly planning — to argue for investment in foundations
- When velocity is dropping and the team blames "tech debt" without specifics
- Before a fundraise — investors ask about technical risk
- When a strategy is stalling and the root cause might be architectural

## Process

### Step 1: Get User Context & Expert Profile

In parallel:
- Call `get_user_context` with `command_name: "tech-debt-review"`, `plugin_name: "stratafy-cto"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert` with `role: "cto"` — returns expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step.

### Step 2: Gather Context

From Step 1 results, filter to **active strategies only**.

For each active owned strategy, in parallel:
- `get_strategy` — full content, health score, alerts
- `list_initiatives` filtered by `strategy_id` — technical initiatives, especially stalled ones
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — technical risks
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — technical assumptions (framework choices, scalability bets, etc.)

Also in parallel (not per-strategy):
- `search_workspace` with query "technical debt infrastructure scalability performance migration" — related context

**Do NOT call `get_workspace_snapshot`, `list_risks`, `list_assumptions`, or `list_initiatives` without filters — these return oversized payloads.**

### Step 3: Categorise Debt

For each piece of identified technical debt or risk, classify:

**By type:**
- **Architectural debt** — Foundational choices that constrain future options (monolith, database schema, API design)
- **Infrastructure debt** — Operational gaps (monitoring, CI/CD, deployment, security)
- **Code debt** — Accumulated shortcuts in implementation (coupling, duplication, missing tests)
- **Knowledge debt** — Bus factor, undocumented systems, tribal knowledge

**By strategic impact:**
- **Blocking** — Directly prevents a strategy from executing (e.g., can't scale to support GTM plan)
- **Slowing** — Increases cost and time of strategic initiatives (e.g., every feature takes 2x due to test gaps)
- **Latent** — Not hurting now but will compound (e.g., no observability before scaling)

### Step 4: Assess Against Strategy

For each owned strategy:
- What technical debt directly threatens this strategy's execution?
- What assumptions about technical capacity underpin this strategy?
- Where has the team taken shortcuts to hit strategic deadlines?
- What debt is being created *right now* by current initiative work?

### Step 5: Present

```
TECH DEBT REVIEW — [Workspace Name]
Date: [today]

━━━ OVERALL POSTURE ━━━━━━━━━━━━━━━━━━━━
Tech debt severity: [Low / Moderate / High / Critical]
Debt creating most drag: [one-line summary]
Strategies at risk: [N] of [total]

━━━ BLOCKING DEBT ━━━━━━━━━━━━━━━━━━━━━━
[Debt item] — [type: architectural/infrastructure/code/knowledge]
  Blocks: [Strategy] → [Initiative]
  Impact: [what can't happen until this is resolved]
  Recommended action: [specific fix]
  Effort estimate: [T-shirt size: S/M/L/XL]

━━━ SLOWING DEBT ━━━━━━━━━━━━━━━━━━━━━━━
[Debt item] — [type]
  Affects: [Strategy] — [how it slows execution]
  Cost of inaction: [what happens if ignored for 6 months]
  Recommended action: [fix]
  Effort: [S/M/L/XL]

━━━ LATENT DEBT ━━━━━━━━━━━━━━━━━━━━━━━━
[Debt item] — [type]
  Risk: [what triggers this becoming blocking]
  Timeline: [when it likely becomes a problem]

━━━ DEBT BEING CREATED NOW ━━━━━━━━━━━━━
[Current initiative] is creating [debt type] because [reason]
  Decision: Accept and track, or invest now to prevent?

━━━ INVESTMENT RECOMMENDATION ━━━━━━━━━━
Priority 1: [Debt item] — [effort] — unlocks [strategic outcome]
Priority 2: [Debt item] — [effort] — prevents [risk]
Priority 3: [Debt item] — [effort] — improves [velocity/reliability]

Recommended allocation: [X]% of next quarter's engineering capacity on debt reduction
```

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "tech-debt-review"
- `_change_reasoning`: brief explanation

## Rules

- **Strategic framing, always.** "This function is poorly written" is code review. "This module's coupling means every GTM channel integration takes 3 weeks instead of 3 days" is CTO-level thinking.
- **Quantify the cost.** Debt without a price tag gets ignored. Express cost in time (days/weeks of delay), risk (what breaks), or opportunity cost (what you can't build).
- **Separate debt from preference.** Not everything the team dislikes is debt. Debt is a choice that has ongoing cost. Ugly code that works and doesn't slow anyone down is not debt.
- **Be honest about debt creation.** Fast-moving startups create debt deliberately. The CTO's job is to make that trade-off visible, not to prevent it.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
