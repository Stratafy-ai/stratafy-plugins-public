# /stratafy-fd:quarterly-review

Produce a comprehensive quarterly financial review that connects financial performance to strategic progress. Designed for leadership meetings, board presentations, or coaching sessions.

## When to Use

- End of quarter financial review
- Preparing for a board meeting with financial content
- Strategy coaching session focused on financial health
- Annual planning that needs Q-over-Q analysis

## Input

Optional:
- **A specific quarter** — "Q4 2025 review"
- **A specific focus** — "quarterly review focused on runway"
- **Nothing** — I'll review the current/most recent quarter

## Process

### Step 1: Gather Comprehensive Data
Pull from multiple sources simultaneously:
- `get_workspace_snapshot` — Full strategic context
- `get_strategy_tree` — Strategy priorities and status
- `get_finance_proposal` — COA and account structure
- `list_finance_mappings` — Strategy-spend connections
- `list_metrics` — All financial KPIs
- `list_objectives` — OKRs with financial relevance
- `list_risks` — Financial risks
- `list_assumptions` — Financial assumptions
- `list_decisions` — Financial decisions made this quarter
- `list_insights` — Financial insights generated

### Step 2: Financial Performance Summary
- Revenue actual vs. plan (if metrics are tracked)
- Expense actual vs. plan
- Burn rate trend over the quarter
- Cash position and runway
- Key metric movements (with trend via `get_metric_trend`)

### Step 3: Strategy-Finance Alignment Analysis
- Run a full financial scan (L1 + L2)
- Compare spend allocation to strategy priorities
- Identify alignment improvements or degradation
- Flag material mismatches

### Step 4: Assumption & Risk Review
- Which financial assumptions held? Which didn't?
- Update confidence levels on financial assumptions
- Review financial risks — any escalated or materialised?
- New risks to add based on Q performance?

### Step 5: Forward View
- Updated runway projection
- Next quarter budget implications
- Investment recommendations (increase/decrease/maintain)
- Decisions needed before next quarter starts

### Step 6: Generate Report

## Output Format

```
QUARTERLY FINANCIAL REVIEW — [Workspace Name]
Period: [Quarter]
Prepared: [Date]

EXECUTIVE SUMMARY
[2-3 sentences: How did we do financially, and what does it mean strategically?]

FINANCIAL PERFORMANCE
  Revenue: [actual] ([vs plan])
  Expenses: [actual] ([vs plan])
  Burn Rate: [current] ([trend])
  Runway: [months] ([change from last quarter])

STRATEGY-FINANCE ALIGNMENT
  Overall Score: [X]% ([change])
  [Strategy 1]: [spend] — [aligned/misaligned] — [commentary]
  [Strategy 2]: [spend] — [aligned/misaligned] — [commentary]

KEY METRICS
  [Metric 1]: [value] ([trend])
  [Metric 2]: [value] ([trend])

ASSUMPTIONS REVIEWED
  Validated: [list]
  Challenged: [list]
  Invalidated: [list]

RISKS
  New: [list]
  Escalated: [list]
  Mitigated: [list]

DECISIONS MADE
  [Decision 1] — [financial impact]

FORWARD VIEW
  Runway: [months at current burn]
  Key risks: [top 2-3]
  Decisions needed: [list]
  Budget recommendations: [increase/decrease/maintain by area]

RECOMMENDATIONS
  1. [Action] — [rationale] — [financial impact]
  2. [Action] — [rationale] — [financial impact]
  3. [Action] — [rationale] — [financial impact]
```

I'll also offer to save this as a document in the workspace using `create_document`.

## For the FD
Board-ready analysis. Numbers with strategic context. I'll draft it, you refine the narrative and present it. Focus areas: what the numbers actually mean for strategy execution, not just what the numbers are.

## For the Finance Team
This is what your day-to-day work adds up to. I'll show you how the categorisations, metric entries, and reconciliations you do every month connect to this quarterly picture. Your work matters — here's the proof.
