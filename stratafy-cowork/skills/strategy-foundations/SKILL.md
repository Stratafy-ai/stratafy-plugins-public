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

### Level 4: Tactics (Execution)

**Question**: How do we deliver and measure progress?

The detailed work of execution:

- **Objectives** — Measurable outcomes (OKR-style), attached to strategies or initiatives
- **Key Results** — Specific targets with current values and confidence scores
- **Metrics** — Ongoing performance indicators with thresholds and polarity
- **Milestones** — Time-bound checkpoints

**Governance**: Team leads + Contributors. Reviews weekly/monthly.

**Key rule**: Plans attach to initiatives, not directly to strategies. Objectives can link to either.

## The Cascade

Strategy flows from abstract to concrete. Each level translates the one above into something more concrete, operates at a faster cadence, and has different governance:

```
Foundation: "We exist to close the gap between intent and execution"
    ↓ constrains
Strategy: "Focus on AI-forward organisations with strategic operating system"
    ↓ allocates resources to
Initiative: "Build coach onboarding workflow" ($200K, Q1-Q2, Team of 3)
    ↓ defines outcomes
Objective: "Onboard 50 coaches in first quarter"
    ↓ measured by
Metrics: "Active coach workspaces: 50, Client retention: 80%"
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
    ↓ drives
Initiatives (strategic work with timelines and owners)
    ↓ measured by
Objectives & Key Results (targets and progress)
    ↓ informed by
Signals → Insights → Decisions
    ↓ tracked by
Risks & Assumptions (what could go wrong, what we're betting on)
```

## Common Anti-Patterns

Watch for these failures and call them out:

- **Strategy-as-to-do-list** — A list of projects is not a strategy. Strategy is about choices: what you will AND won't do.
- **Implicit assumptions** — The most dangerous assumptions are the ones nobody has stated. Surface them.
- **Missing measurement** — If you can't tell whether a strategy is working, it's not a strategy, it's a hope.
- **Orphaned initiatives** — Work that doesn't connect to any strategy. Ask: why are we doing this?
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
