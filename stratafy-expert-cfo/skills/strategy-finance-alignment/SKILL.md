# Strategy-Finance Alignment

You help users connect financial reality to strategic intent. This is the bridge between "what we say matters" and "where we actually spend money." It's the most honest mirror a company has.

## The Core Question

> Does the way we spend money match the way we say we'll win?

If a company says Product is their #1 strategy but 60% of spend goes to Sales, there's a misalignment. It might be intentional (invest in sales now, product later) or unconscious (strategic drift). Either way, making it visible is the highest-value financial activity.

## Strategy-to-Account Mapping

### How Mapping Works

Every postable account in the COA can map to one or more strategies:

- **Primary mapping** — This strategy is the main reason this cost exists
- **Supporting mapping** — This strategy benefits from this cost but isn't the primary driver
- **Weight** — What proportion of the account's spend is attributed to this strategy (0.0 to 1.0)
- **Rationale** — Why this mapping exists (critical for auditability)

Use `create_finance_mapping` to create mappings. Always include rationale.

### Mapping Patterns

**One-to-one:** Account serves exactly one strategy.
```
5110 Engineering Salaries → Product Architecture (primary, weight: 1.0)
Rationale: "All engineering headcount builds product capabilities"
```

**One-to-many:** Account serves multiple strategies.
```
5220 SaaS Tools → Product Architecture (primary, weight: 0.6)
                → GTM (supporting, weight: 0.3)
                → Operations (supporting, weight: 0.1)
Rationale: "Dev tools are 60% of SaaS spend, sales tools 30%, ops tools 10%"
```

**Unmapped:** Account has no strategy connection.
```
5900 Miscellaneous Expenses → ???
Action: Either map it or question whether the spend should exist
```

### Mapping Quality Checks

1. **Weight coherence** — Weights for a single account should sum to ~1.0
2. **Rationale completeness** — Every mapping should have a "why"
3. **Freshness** — Mappings should be reviewed when strategy changes
4. **Bidirectional coverage** — Every strategy should have financial backing; every account should have strategic justification

## Strategic Spend Analysis

### Building the View

To analyse strategic spend allocation:

1. `get_strategy_tree` — Get all strategies with their priorities and status
2. `get_finance_proposal` — Get the COA with all accounts
3. `list_finance_mappings` — Get all strategy-account connections
4. Cross-reference: for each strategy, sum the weighted account allocations

### What to Look For

**Alignment signals (good):**
- High-priority strategies have proportionally high financial backing
- New strategic bets have dedicated funding (not buried in existing accounts)
- Cost reductions target low-priority or completed strategies

**Misalignment signals (concerning):**
- Top strategy by priority is bottom strategy by spend
- "Critical" initiatives have no dedicated budget line
- Spend is growing fastest in non-strategic areas
- Legacy costs persist for cancelled strategies

**Drift signals (urgent):**
- Unmapped spend growing quarter over quarter
- New accounts created without strategy mappings
- Budget increases in areas not connected to any active strategy

### Presenting Findings

For the FD (peer voice):
> "Three findings from the Q1 spend analysis:
> 1. GTM accounts for 45% of spend but is priority #3. Product is priority #1 at 22% of spend. Either the strategy is wrong or the budget is.
> 2. R120K/month in unmapped SaaS tools — that's 8% of burn with no strategic justification.
> 3. The Finance Strategy has zero dedicated funding. Everything is shared allocations. If finance matters, it needs its own budget line."

For the team (coach voice):
> "Let's look at what the numbers tell us about our priorities. See how the GTM strategy has the biggest budget? That means the company is investing most heavily in going to market. But notice that the strategy tree says Product is priority #1. That's a useful tension to flag to the FD — it might be intentional, or it might be something to discuss."

## Connecting to Decisions

Financial misalignment often surfaces decisions that need to be made. When you find misalignment:

1. Document the finding as an insight (`create_insight`)
2. If it requires a choice, create a decision (`create_decision`)
3. Link the decision to the relevant strategy
4. Include financial data in the decision context

Example:
> **Decision:** Reallocate 15% of GTM budget to Product Engineering
> **Context:** GTM is 45% of spend but priority #3. Product is priority #1 at 22%.
> **Options:** (A) Reallocate now, (B) Reallocate next quarter, (C) Re-prioritise strategies to match current spend
> **Financial impact:** R180K/month shift. GTM headcount implications.

## Connecting to Assumptions

Financial plans rest on assumptions. Surface and track them:

- **Revenue assumptions** — Growth rate, churn, pricing, market size
- **Cost assumptions** — Headcount plan, infrastructure scaling, vendor pricing
- **Funding assumptions** — Runway, fundraise timing, revenue targets for self-sustainability
- **Market assumptions** — Willingness to pay, competitive pricing, cost structure advantages

Use `create_assumption` to log financial assumptions and `link_assumption_to_context` to connect them to the strategies they underpin.

## Connecting to Risks

Financial analysis naturally surfaces risks:

- Runway shorter than strategy timeline → `create_risk` (type: financial)
- Revenue concentrated in one customer → `create_risk` (type: market)
- Cost growing faster than revenue → `create_risk` (type: execution)
- Strategy unfunded → `create_risk` (type: strategic)

Always connect financial risks to the risk register so they're visible alongside operational and market risks.

## Quarterly Alignment Review

Every quarter, run the full alignment cycle:

1. **Refresh mappings** — Have any strategies changed? Do mappings still hold?
2. **Calculate alignment scores** — L1 structural, L2 P&L, L3 balance sheet
3. **Trend analysis** — Is alignment improving or degrading?
4. **Variance analysis** — Planned vs. actual spend by strategy
5. **Forward view** — Does next quarter's budget reflect updated strategic priorities?
6. **Assumption check** — Are financial assumptions still valid?
7. **Report** — Produce a strategy-finance alignment report

## When to Use This Knowledge

Activate this skill when:
- A user asks about budget allocation relative to strategy
- Someone wants to understand where money is going and why
- Financial misalignment or drift is suspected
- Quarterly or annual budget planning is happening
- A strategy change needs financial impact assessment
- Someone asks "are we putting our money where our mouth is?"
