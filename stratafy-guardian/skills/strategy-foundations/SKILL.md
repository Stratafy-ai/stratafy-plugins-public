---
name: strategy-foundations
description: Teach how strategy actually works using first principles that ground every recommendation, workspace, and review
---

# Strategy from First Principles

You are working with Stratafy — the operating system for strategy. This skill teaches you how strategy actually works, not just how Stratafy's UI is laid out. Every recommendation, every workspace you build, every review you run should be grounded in these principles.

## The Seven Questions

Strategy execution must answer seven fundamental questions. Everything in Stratafy maps to one of these:

| Question              | Answer                   | What It Contains                                  |
| --------------------- | ------------------------ | ------------------------------------------------- |
| Who are we?           | Foundation (Identity)    | Mission, vision, values, principles, beliefs      |
| Where do we focus?    | Strategy (Direction)     | Strategic themes, positioning, capabilities       |
| What do we commit to? | Initiatives (Commitment) | Resourced programs with budgets, teams, timelines |
| How do we execute?    | Tactics (Execution)      | Objectives, plans, milestones, metrics            |
| How do we learn?      | Cortex (Intelligence)    | Assumptions, risks, insights, alignment scoring   |
| What informs us?      | Context (Reality)        | Documents, market data, signals, radar            |
| What supports us?     | Tools (Capabilities)     | Integrations, agents, automations                 |

The first four are **execution levels** (the Strategy Stack). The last three are **enablers** that cut across all levels.

## The Strategy Stack (4 Execution Levels)

### Level 1: Foundation (Identity)

**Question**: Who are we?

The enduring pillars that anchor all decisions:

- **Mission** — Why we exist
- **Vision** — The future we're creating
- **Values** — How we make tradeoffs (not platitudes — operational commitments)
- **Principles** — How we operate (optional)
- **Beliefs** — What we hold true about our market/world (optional but high-value)

**Governance**: CEO / Founders. Changes rarely — measured in years.

**Quality test**: If a value doesn't help you make a hard tradeoff, it's a platitude. "We value excellence" tells you nothing. "We ship fast over ship perfect" tells you everything.

### Level 2: Strategy (Direction)

**Question**: Where do we focus and how do we win?

High-level choices about direction:

- **Strategic themes** — Areas of organisational focus
- **Positioning** — Where and how we compete
- **Capability requirements** — What we must build to win

**Governance**: Leadership team. Reviews annually.

**Critical distinction**: Strategies do NOT have budgets or team allocations. They express direction only. If it has a budget, it's an initiative.

**Anti-pattern**: If you have three corporate strategies, you have none. Corporate strategy is singular — one overarching direction. Functional strategies (GTM, Product, People, etc.) sit beneath it.

**Strategy types in Stratafy**: `corporate`, `business_unit`, `functional`
**Statuses**: `draft`, `active`, `under_review`, `completed`, `cancelled`

### Level 3: Initiatives (Commitment)

**Question**: What programs are we betting on?

Initiatives are the bridge between strategy and execution. They represent concrete, resourced commitments:

- **Named programs** with clear ownership
- **Budget allocation** — What we're spending
- **Team assignment** — Who's working on it
- **Timeline** — Start and end dates
- **Success criteria** — How we know we're done

**Governance**: Leadership + Department heads. Reviews quarterly.

**Critical distinction**: Initiatives ARE the commitment layer. Previous frameworks conflated direction with commitment — "strategy" meant both "where to focus" and "what to fund." Stratafy separates them. This enables strategies to remain stable while initiatives adapt.

**Initiative types**: `strategic` (moves the needle on strategy), `tactical` (enables strategic work), `operational` (keeps the lights on)

**Anti-pattern**: Operational creep — when most initiatives are operational, strategy is being neglected. Every initiative should trace to a strategy.

### Level 4: Objectives & Metrics (Measurement)

**Question**: Are we achieving what we set out to achieve? Are we healthy while doing it?

A strategy needs two parallel instruments: **objectives** tell you whether you're winning, **metrics** tell you whether you're healthy. They are parallel — not hierarchical. Objectives close. Metrics persist.

#### Objective Types

**Strategic Objective** — lives on the **strategy**.
Answers: "What does winning look like for this strategy?"
The outcome a strategy exists to achieve. Bounded — you can say "we achieved this" or "we didn't." Every active strategy should have at least one. Should be few (3–4 max per strategy).

*Example*: "Platform supporting 3+ simultaneous customer deployments reliably."

*Quality test*: If you can't tell whether it's been achieved, it's not an objective — it's an aspiration. "Be the best" fails. "Win 3 enterprise accounts in fintech" passes.

**Key Result** — lives on the **initiative**.
Answers: "How do we know this initiative is delivering?"
Measurable evidence that an initiative is delivering toward its parent strategy's objectives. Must be specific and measurable (quantitative or binary). Should trace to a strategic objective — you should be able to say which strategic objective this key result serves. Supports OKR scoring (0–1).

*Example*: "Context assembly time under 200ms with 80% cache hit rate" (serves the strategic objective "AI agent has full strategic context in under 200ms").

*Quality test*: If a key result doesn't trace to a strategic objective, ask: what is this initiative actually proving? If you can't answer, the initiative may be orphaned work.

**Milestone** — lives on the **initiative**.
Answers: "Have we reached this checkpoint?"
Binary progress markers. Done or not done. Marks that work is complete, not that it's generating strategic value (that's what key results are for).

*Example*: "Phase 1: Manual briefing prototype live and tested for 7 consecutive days."

*Quality test*: If a milestone has a percentage or range, it's a key result, not a milestone.

#### Objective Hierarchy

```
Strategy
  ├── Strategic Objective (what winning looks like)
  │     ↑ key results should trace to these
  ├── Metric (leading indicator — continuous, never closes)
  ├── Metric (lagging indicator — continuous, never closes)
  └── Initiative
        ├── Key Result (measurable evidence of delivery)
        └── Milestone (binary checkpoint of delivery)
```

| | Strategic Objective | Key Result | Milestone |
|---|---|---|---|
| **Lives on** | Strategy | Initiative | Initiative |
| **Answers** | What does winning look like? | Is this initiative delivering? | Have we reached this checkpoint? |
| **Measurement** | Quantitative, qualitative, or binary | Quantitative or binary | Binary only |
| **Closes?** | Yes — achieved or not | Yes — scored 0–1 | Yes — done or not |
| **Proves** | Strategy success | Initiative contribution to strategy | Work completion |

#### Metrics — Parallel, Not Subordinate

Metrics are the strategy's vital signs. They don't feed objectives and objectives don't feed them. They're a separate instrument:

- **Leading metrics** — where you have agency (e.g. pipeline size, demos completed, content published)
- **Lagging metrics** — confirm what already happened (e.g. MRR, retention rate, conversion rate)

Every active strategy should have at least one leading and one lagging metric. Every metric **must** be linked to the strategy it measures. Unlinked metrics are orphaned data — a number without strategic context.

**Governance**: Team leads + Contributors. Reviews weekly/monthly.

**Anti-patterns**:
- **Vanity metrics** — Numbers that go up but don't indicate strategic progress. "Page views" without conversion context is vanity.
- **Orphaned metrics** — Metrics with no strategy link. If it doesn't measure a strategy, why are we tracking it?
- **Unmeasured strategies** — Strategies with no objectives and no metrics. If you can't tell whether it's working, it's not a strategy — it's a hope.
- **Key results without a claim** — Key results on initiatives that don't trace to any strategic objective. Evidence without a hypothesis.

## The Cascade

Strategy flows from abstract to concrete. Each level translates the one above into something more concrete, operates at a faster cadence, and has different governance:

```
Foundation: "We exist to close the gap between intent and execution"
    ↓ constrains
Strategy: "Focus on AI-forward organisations with strategic operating system"
    ├── Strategic Objective: "Win 3 enterprise lighthouse customers"
    ├── Leading Metric: "Qualified pipeline size"
    ├── Lagging Metric: "Active lighthouse customers"
    ↓ allocates resources to
Initiative: "Build coach onboarding workflow" ($200K, Q1-Q2, Team of 3)
    ├── Key Result: "50 coaches onboarded" (traces to strategic objective)
    └── Milestone: "Onboarding flow v1 shipped"
```

When reviewing any entity, check upward: does this initiative actually serve a strategy? Does this strategy align with the foundation? Misalignment at any level means the cascade is broken.

## The Intelligence Layers

Surrounding the four execution levels are five intelligence layers that make strategy a living system:

### Assumptions

Every strategy rests on assumptions — beliefs about the market, customer, technology, or competition that haven't been fully validated. Making these explicit is the single highest-value activity in strategy work.

- **Confidence levels**: `hypothesis` (untested), `likely` (some evidence), `validated` (proven)
- **Impact if wrong**: `low`, `medium`, `high`, `critical`
- High-impact, low-confidence assumptions should be tested first

### Risks

Things that could derail the strategy. Each risk has likelihood, impact, and mitigation ideas. Risks link to strategies and initiatives.

- Track status: `identified`, `mitigated`, `occurred`, `closed`
- Risks that have occurred become lessons (insights)

### Decisions

Choices already made or still open. Documenting decisions with their context prevents relitigating settled questions and surfaces pending choices that block progress.

- **Status**: `pending` (needs deciding), `decided` (resolved), `deferred` (parked)
- Include rationale — future-you needs to know WHY, not just WHAT

### Insights

Knowledge gained from experience, research, or analysis. Insights feed future decisions and strategy adjustments.

### Signals

External or internal indicators worth watching — competitive moves, market shifts, customer behaviour changes, technology developments. Signals flow through a pipeline: detect → classify → research → assess impact → route to owner → act.

## How Entities Connect in Stratafy

```
Foundation (mission, vision, values, principles, beliefs)
    ↓ anchors
Strategy Tree (corporate → functional → sub-strategies)
    ├── Strategic Objectives (what winning looks like)
    ├── Metrics (leading + lagging vital signs, linked to strategy)
    ↓ drives
Initiatives (strategic work with timelines and owners)
    ├── Key Results (measurable evidence, traces to strategic objective)
    ├── Milestones (binary checkpoints)
    ↓ informed by
Signals → Insights → Decisions
    ↓ tracked by
Risks & Assumptions (what could go wrong, what we're betting on)
```

## Common Anti-Patterns

Watch for these failures and call them out:

- **Strategy-as-to-do-list** — A list of projects is not a strategy. Strategy is about choices: what you will AND won't do.
- **Implicit assumptions** — The most dangerous assumptions are the ones nobody has stated. Surface them.
- **Missing measurement** — If you can't tell whether a strategy is working, it's not a strategy, it's a hope. Check: does every active strategy have at least one objective AND at least one metric?
- **Orphaned initiatives** — Work that doesn't connect to any strategy. Ask: why are we doing this?
- **Orphaned metrics** — Metrics with no strategy link. A number without strategic context is noise.
- **Key results without a claim** — Key results on initiatives that don't trace to a strategic objective. Ask: what strategic outcome does this evidence support?
- **Strategies without vital signs** — Strategies with no leading or lagging metrics. You're flying blind.
- **Values as platitudes** — "Integrity, excellence, teamwork" — every company claims these. Real values are tradeoffs.
- **Confusing strategy with tactics** — "Hire 10 engineers" is a tactic. "Build a world-class engineering capability" is strategy.
- **Operational creep** — When execution crowds out strategy. If everything is urgent, nothing is strategic.
- **Static strategy** — Strategy that lives in a document nobody reads. Strategy must be a living system with continuous sensing (Radar + Pulse).

## Working with Workspaces

- Always start by selecting a workspace with `select_workspace`
- Use `get_workspace_snapshot` for a quick overview of the entire strategic landscape
- The strategy tree (`get_strategy_tree`) shows the full hierarchy in one call
- Foundation elements (mission, vision, values) are read with dedicated tools
- `workspace_context` stores structured company info (industry, stage, market) that powers radar scans and coherence checks

## When to Use This Knowledge

Activate this skill when:

- A user asks "what is..." or "how does... work" about strategy concepts
- Someone is exploring a workspace for the first time
- You need to explain the relationship between entities
- A user seems confused about where something belongs in the strategic architecture
- You're coaching someone through strategy definition
- You spot an anti-pattern (strategy-as-to-do-list, orphaned initiatives, implicit assumptions)
- Someone conflates strategy with initiatives or tactics
