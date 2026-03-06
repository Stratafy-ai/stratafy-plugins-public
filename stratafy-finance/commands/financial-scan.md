# /stratafy-finance:financial-scan

Run a financial alignment scan to measure how well the company's finances connect to its strategy. Produces an alignment score with specific findings and recommendations.

## When to Use

- Monthly check-in on financial-strategic alignment
- After budget changes or strategy updates
- Before a quarterly review or board meeting
- When you suspect money isn't going where strategy says it should

## Input

Optional:
- **A specific strategy** — "scan alignment for our GTM strategy"
- **A scan level** — "run an L2 P&L scan"
- **Nothing** — I'll run a full L1 structural scan

## Process

### Step 1: Gather Financial Data
I'll pull data from multiple sources simultaneously:
- `list_finance_proposals` — Find the active/approved COA
- `get_finance_proposal` — Get full account structure
- `list_finance_mappings` — Get all strategy-account connections
- `get_strategy_tree` — Get strategy priorities and status

### Step 2: L1 Structural Scan
Baseline alignment check:
- Count postable accounts vs. mapped accounts
- Identify unmapped accounts
- Check mapping weight coherence (do weights sum correctly?)
- Check for strategies with zero financial backing
- Calculate structural alignment score

### Step 3: L2 P&L Analysis (if requested or L1 score warrants it)
Deeper analysis:
- Group expenses by strategy using mappings and weights
- Compare spend allocation to strategy priority ranking
- Identify mismatches (high-priority strategy, low spend or vice versa)
- Flag spend drift since last scan

### Step 4: Synthesise Findings
Produce findings ranked by severity:
- **Critical** — Unmapped spend >20%, or top strategy unfunded
- **Warning** — Alignment score declining, or material mismatches
- **Info** — Minor gaps, optimization opportunities

### Step 5: Recommendations
Specific, actionable next steps:
- "Map these 8 accounts to close the structural gap"
- "Investigate R45K/month in miscellaneous — likely belongs to Operations"
- "GTM spend is 3x Product spend but Product is priority #1 — decision needed"

## Output Format

```
FINANCIAL ALIGNMENT SCAN — [Workspace Name]
Date: [today]
COA: [Proposal name] ([status])

L1 STRUCTURAL SCORE: [X]%
  Postable accounts: [N]
  Mapped accounts: [N]
  Unmapped accounts: [N]

STRATEGY COVERAGE:
  [Strategy 1]: [N] accounts, [X]% of mapped spend
  [Strategy 2]: [N] accounts, [X]% of mapped spend
  ...
  Unfunded strategies: [list]

FINDINGS:
  1. [CRITICAL/WARNING/INFO] — [Finding]
  2. [CRITICAL/WARNING/INFO] — [Finding]

RECOMMENDATIONS:
  1. [Action] — [Expected impact]
  2. [Action] — [Expected impact]

TREND: [Improving/Stable/Degrading] since last scan
```

## For the FD
Direct findings, no padding. You'll see the number, what it means, and what to do about it. I'll flag the governance issues, not just the accounting gaps.

## For the Finance Team
I'll explain what each finding means for the work you do day-to-day. When I say an account is "unmapped," I'll show you how to fix it and why it matters for the bigger picture.
