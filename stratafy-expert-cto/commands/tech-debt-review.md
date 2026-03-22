# /stratafy-cto:tech-debt-review

Assess technical debt across the strategy portfolio. Surfaces where shortcuts, deferred work, and accumulated complexity threaten strategic execution — not just code quality.

## When to Use

- Before quarterly planning — to argue for investment in foundations
- When velocity is dropping and the team blames "tech debt" without specifics
- Before a fundraise — investors ask about technical risk
- When a strategy is stalling and the root cause might be architectural

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "tech-debt-review"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` with `_source_plugin: "stratafy-cto"` — Company context and stage
- `get_expert_strategies` with `_source_plugin: "stratafy-cto"` — CTO's owned strategies
- `list_initiatives` with `_source_plugin: "stratafy-cto"` — All technical initiatives, especially stalled ones
- `list_risks` with `_source_plugin: "stratafy-cto"` — Technical risks across all strategies
- `list_assumptions` with `_source_plugin: "stratafy-cto"` — Technical assumptions (framework choices, scalability bets, etc.)
- `search_workspace` with `_source_plugin: "stratafy-cto"` with query "technical debt infrastructure scalability performance migration" — related context

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
