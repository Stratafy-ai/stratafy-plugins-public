---
name: financial-intelligence
description: Design, analyse, and maintain financial architecture connected to strategic intent
---

# Financial Intelligence

You help users design, analyse, and maintain the financial architecture of their organisation — with everything connected to strategic intent. This skill covers chart of accounts design, financial alignment scanning, and financial modelling.

## Chart of Accounts (COA) Design

The COA is the financial backbone. In Stratafy, it's not just an accounting structure — it's a strategic lens. Every account should tell you which strategy it serves.

### COA Design Principles

1. **Strategy-first structure** — Group accounts by strategic function, not just accounting convention
2. **Every account maps** — If an account can't trace to a strategy, question why it exists
3. **Readable by non-accountants** — Account names should make strategic sense to a CEO, not just a bookkeeper
4. **Right granularity** — Enough detail to see strategic patterns, not so much that it's noise

### Account Types
- `asset` — What the company owns (cash, equipment, IP)
- `liability` — What the company owes (loans, payables)
- `equity` — Owner's stake
- `revenue` — Income streams
- `expense` — Operating costs
- `cogs` — Cost of goods sold / direct costs

### Cost Nature Classification
- `fixed` — Doesn't change with volume (rent, salaries)
- `variable` — Scales with activity (cloud usage, commissions)
- `semi_variable` — Has a fixed base + variable component
- `one_time` — Non-recurring (equipment purchase, setup costs)

### Building a COA

Use the proposal workflow — never build a COA directly in production:

1. `create_finance_proposal` — Create a draft proposal with strategic narrative
2. `create_finance_account` — Add accounts one by one, or use `bulk_create_finance_accounts` for batch
3. `create_finance_mapping` — Map each account to its parent strategy
4. Review the complete proposal with `get_finance_proposal`
5. Update status to `proposed` when ready for review
6. Update to `approved` when the human confirms

### Importing from Existing Systems

If the company has an existing COA (from Xero, QuickBooks, Sage, etc.):

1. Get the spreadsheet exported from their accounting system
2. Use `import_sheet_as_finance_proposal` to import with column mapping
3. Review imported accounts — many will need strategic mapping
4. Add missing strategic context and mappings
5. Propose structure improvements that better reflect strategy

### Header vs. Postable Accounts

- **Header accounts** (`is_header: true`) — Grouping accounts that contain sub-accounts. Not posted to directly.
- **Postable accounts** — Where actual transactions land. These get strategy mappings.

```
5000 Operating Expenses (header)
  5100 People Costs (header)
    5110 Engineering Salaries (postable → maps to Product Architecture strategy)
    5120 Sales Salaries (postable → maps to GTM strategy)
  5200 Technology (header)
    5210 Cloud Infrastructure (postable → maps to Product Architecture strategy)
    5220 SaaS Tools (postable → maps to multiple strategies)
```

## Financial Alignment Scanning

Alignment scans measure how well the financial structure connects to strategy. Three levels:

### L1 — Structural Scan
- Does every postable account have at least one strategy mapping?
- Are mapping weights balanced (sum to ~1.0 per account)?
- Are there orphaned accounts with no strategic connection?
- **Output:** Structural alignment score + list of unmapped accounts

### L2 — P&L Scan
- Does revenue mix match strategic priorities?
- Does expense allocation reflect strategic emphasis?
- Are high-priority strategies adequately funded?
- Are low-priority strategies consuming disproportionate resources?
- **Output:** P&L alignment matrix showing strategy vs. spend

### L3 — Balance Sheet Scan
- Do asset investments align with strategic capabilities needed?
- Does the liability structure support the strategy timeline?
- Is working capital adequate for execution plans?
- **Output:** Balance sheet alignment assessment

### Running Scans

There's no single "run financial scan" MCP tool yet. Instead, synthesise from available data:

1. `get_finance_proposal` — Get the full COA with accounts and mappings
2. `list_finance_mappings` — Review all strategy-account connections
3. `get_strategy_tree` — Compare against strategic priorities
4. Calculate alignment: mapped accounts / total postable accounts
5. Identify gaps: strategies with no financial backing, accounts with no strategy

### Interpreting Scan Results

- **>80% mapped** — Strong financial-strategic alignment
- **60-80% mapped** — Gaps exist, likely in operational or legacy accounts
- **<60% mapped** — Financial structure is disconnected from strategy. Major red flag.

For the FD: "Your L1 score is 62%. 38% of spend has no strategic justification. Here are the 12 accounts that need mapping."

For the team: "Great progress — we've mapped 62% of accounts to strategies. Let's work through the remaining 12 accounts together. I'll explain which strategies each one likely supports."

## Financial Metrics

Track financial KPIs using Stratafy's metric system:

### Common Financial Metrics
- **Monthly Burn Rate** — Total monthly cash outflow
- **Runway** — Months of cash remaining at current burn
- **Revenue Growth Rate** — MoM or QoQ revenue change
- **Gross Margin** — Revenue minus COGS as percentage
- **Strategic Spend Ratio** — % of spend mapped to active strategies
- **Customer Acquisition Cost (CAC)** — Cost per new customer
- **Lifetime Value (LTV)** — Revenue per customer over lifetime
- **LTV:CAC Ratio** — Unit economics health

Use `create_metric` to set up tracking and `record_metric_value` to log values.

## Financial Risk Patterns

Common financial risks to watch for and log via `create_risk`:

- **Runway risk** — Cash runs out before strategy milestones
- **Concentration risk** — Revenue depends on too few customers/segments
- **Cost escalation** — Variable costs growing faster than revenue
- **Currency risk** — Revenue and costs in different currencies
- **Funding gap** — Strategy requires investment the company can't fund
- **Unmapped spend drift** — Growing proportion of spend without strategic justification

## When to Use This Knowledge

Activate this skill when:
- A user wants to design or review a chart of accounts
- Someone asks about financial alignment or strategic spend
- The conversation involves COA proposals, accounts, or mappings
- Someone wants to understand financial health relative to strategy
- Financial scan results need interpretation
- A user is importing or restructuring their accounting system
