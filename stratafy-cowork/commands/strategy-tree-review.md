# /stratafy:strategy-tree-review

Review a strategy branch for structural quality and completeness. This command walks through each layer of the strategy cascade — strategies, initiatives, then objectives and metrics — checking whether the tree is well-defined as a piece of strategic architecture.

This is not an alignment scan or an execution health check. Those are handled by `/stratafy:strategy-review` and the alignment/coherence scan tools. This command answers a different question: **is the tree well-built?** If you handed this branch to a new team member or a strategy coach, would they say "this is clearly defined" or "this has gaps and things at the wrong level"?

## When to Use

- After building or restructuring a strategy branch
- During quarterly planning to sanity-check the structure before committing
- When onboarding a new client and reviewing their existing strategy setup
- When something feels "off" but you can't pinpoint what's missing
- Before running alignment scans — fix structural issues first so the scans are meaningful

## Process

### Step 1: Load the Top Two Levels and Ask

Pull the strategy tree at a shallow depth to give the user a map of the workspace:

- `get_strategy_tree` with `max_depth: 2`, `include: ["strategies"]`, `detail: "minimal"` — just names, types, and status

If the enhanced tree tool isn't available, fall back to:
- `list_strategies` — get all strategies
- For each top-level strategy, call `get_strategy_tree` with `max_depth: 2` and `include_objectives: false`

Present the top two levels as a simple tree and ask which branch to review:

```
Here's your strategy tree (top two levels):

Corporate: AI-Forward Strategic Operating System
  ├── Product Strategy — Build the platform
  ├── GTM Strategy — Win the mid-market
  ├── People Strategy — Attract and retain
  └── Funding Strategy — Series A readiness

Which branch would you like me to review? I'll do a full structural audit of the strategy and everything beneath it — sub-strategies, initiatives, objectives, and metrics.
```

Wait for the user to choose. They might pick one branch, multiple branches, or say "all." If they pick multiple or all, work through each branch sequentially — present findings for one before moving to the next.

### Step 2: Load the Selected Branch

Once the user has chosen, pull everything for that branch:

- `get_strategy_tree` on the selected strategy with `include: ["strategies", "initiatives", "objectives", "metrics"]`, `detail: "summary"` — full hierarchy with enough detail to assess structure without loading full content
- `get_structure_diagnostics` on the selected strategy — if available, this gives server-side structural analysis (coverage matrix, orphans, gaps, type mismatches)

If the enhanced tools aren't available, fall back to:
- `get_strategy_tree` on the selected strategy with `include_objectives: true`
- `list_metrics` — all metrics in the workspace (to check connections and orphans)

### Step 3: Strategy Layer

Walk the strategy and its sub-strategies, checking whether the structure is sound.

**Is it actually strategy?**
Strategies express direction — where to focus and how to win. They do NOT have budgets or team allocations. If something reads like a commitment with resources and timelines, it's probably an initiative that's been misclassified as a strategy. Look for this — it's one of the most common structural mistakes.

Similarly, check that strategies aren't disguised to-do items. "Hire 10 engineers" is an initiative. "Build world-class engineering capability" is a strategy. If strategy names read like tasks, flag it.

**Shape and balance**
- Single-child branches: a strategy with only one sub-strategy usually means the layer is unnecessary, or a sibling is missing
- Over-branching: more than 5-7 sub-strategies under one parent likely needs grouping
- Uneven depth: some branches deeply nested while others are flat often signals uneven strategic thinking
- At the corporate level, there should ideally be one corporate strategy — if there are multiple, that's worth flagging ("if you have three corporate strategies, you have none")

**Naming and clarity**
- Could someone unfamiliar with the organisation read the tree and understand the intent?
- Are names distinct from each other, or do they blur together?
- Do the sub-strategies collectively cover the parent's intent, or are there obvious directional gaps?

**Types and classification**
- Are strategy types set correctly? (`corporate`, `business_unit`, `functional`)
- Does the type match the position in the hierarchy?

### Step 4: Initiatives Layer

For each strategy in the branch, review its initiatives. Initiatives are the commitment layer — they bridge strategy to execution with resourced programs.

**Linkage**
- Are there strategies with no initiatives? A strategy without initiatives is direction without commitment — it's a wish. Every active strategy should have at least one initiative driving it forward.
- Does each initiative clearly serve its parent strategy? Could you explain the connection in one sentence?

**Classification**
- Are initiative types set? (`strategic`, `tactical`, `operational`)
- Do the types make sense? A "strategic" initiative should be a genuine bet that moves the needle. A "tactical" initiative enables strategic work. An "operational" initiative keeps the lights on.
- Check for operational creep — if most initiatives under a strategic branch are operational, the strategy is being neglected. This is one of the most common and most damaging anti-patterns.

**Definition quality**
- Do initiatives have descriptions that explain what they actually are?
- Do they have start dates and target completion dates?
- Are priorities assigned? Do the relative priorities make sense — are the strategic bets prioritised higher than the operational work?

**Overlaps and redundancy**
- Are multiple initiatives doing essentially the same thing, possibly under different strategies?
- Could any be consolidated?

### Step 5: Objectives & Metrics Layer

Objectives and metrics are the measurement system — they sit together at the execution level of the cascade. Review them as a unified layer.

**Objective quality**
- Are quantitative objectives actually measurable? Check for `target_value` and `current_value`. An objective that says "increase revenue" with no target number isn't measurable.
- Are objectives time-bound? Check `target_date`. Open-ended objectives are hopes, not targets.
- For qualitative objectives — are they specific enough that you could assess progress?
- For binary objectives — are completion criteria clearly defined?

**OKR structure** (if the workspace uses OKR patterns)
- Do strategic objectives have key results beneath them?
- Are key results quantitative and time-bound?
- 3-5 key results per objective is the sweet spot — fewer than 2 may be underspecified, more than 5 is probably too granular

**Coverage gaps**
- Are there strategies with no objectives at all? Every active strategy needs at least one way to measure whether it's working.
- Are there initiatives with no objectives? How will you know if the initiative succeeded?
- Is there an imbalance — some strategies have many objectives while siblings have none?

**Metric connections**
- Are there objectives (especially quantitative ones and key results) with no corresponding metric to track them?
- Are there orphaned metrics — metrics that exist but aren't connected to any objective or strategy?
- Is there a mix of leading indicators (predict future outcomes) and lagging indicators (confirm results)? An objective tracked only by lagging indicators means you'll know you failed only after the fact.

**Metric definition**
- Do metrics have target values and thresholds (green/yellow/red)?
- Is polarity set correctly (higher-is-better vs lower-is-better)?
- Are current values populated, or are the metrics empty shells?

### Step 6: Present Findings

Present the review in conversation. Group findings by layer, following the cascade order. For each finding, flag it as:

- **Issue** — structurally wrong, should be fixed
- **Gap** — something missing that should exist
- **Observation** — worth noting, not necessarily wrong

```
STRATEGY TREE REVIEW — [Strategy Name]
Date: [today]

STRUCTURAL HEALTH: [Strong / Needs Work / Significant Issues]
[1-2 sentence summary]

━━━ STRATEGIES ━━━
[Findings: shape, classification, naming, directional gaps]

━━━ INITIATIVES ━━━
[Findings: linkage, types, operational creep, definition quality]

━━━ OBJECTIVES & METRICS ━━━
[Findings: quality, coverage, connections, leading/lagging balance]

━━━ PRIORITY FIXES ━━━
Recommended improvements, in order:
1. [What to fix and why it matters]
2. [...]
3. [...]

━━━ QUICK WINS ━━━
Things that can be fixed right now:
- [e.g., "Add target dates to 3 objectives missing them"]
- [e.g., "Reclassify initiative X from strategic to operational"]
```

Keep the tone constructive — the goal is to improve the tree, not grade it. When flagging issues, always suggest what to do about them.

After presenting, offer to help fix things: "Want me to start on any of these? I can work through the priority fixes or knock out the quick wins."

## What This Command Does NOT Do

This command focuses on structural quality. It deliberately skips:

- **Foundation alignment** — use `run_alignment_foundation_scan` for that
- **Cross-strategy coherence** — use `run_alignment_coherence_scan` for that
- **Risk and assumption analysis** — those are intelligence layers, not structure
- **Signal processing** — environmental scanning, not tree architecture
- **Execution progress** — OKR scores, initiative completion status (this is about structure, not status)
- **Creating workspace documents** — findings go straight into conversation for fast iteration
