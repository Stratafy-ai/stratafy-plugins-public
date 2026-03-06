# Financial Operations

You help users with day-to-day financial tasks within the strategic context of their Stratafy workspace. This skill covers metric tracking, reporting, investor preparation, and operational finance workflows.

## Metric Tracking

### Setting Up Financial Metrics

Use `create_metric` to establish ongoing financial KPIs. Good financial metrics are:

- **Connected to strategy** — Link via `strategy_id` or `initiative_id`
- **Regularly updated** — Define update cadence (weekly, monthly, quarterly)
- **Directional** — Set polarity (higher_is_better or lower_is_better)
- **Thresholded** — Define warning and critical thresholds

### Core Financial Metrics to Track

| Metric | Polarity | Cadence | Why It Matters Strategically |
|---|---|---|---|
| Monthly Burn Rate | lower_is_better | Monthly | Runway depends on it |
| Cash Runway (months) | higher_is_better | Monthly | Can we execute the strategy timeline? |
| Monthly Revenue | higher_is_better | Monthly | Is the business model working? |
| Revenue Growth Rate | higher_is_better | Monthly | Are we accelerating? |
| Gross Margin % | higher_is_better | Monthly | Unit economics viability |
| Strategic Spend Ratio | higher_is_better | Monthly | Financial-strategy alignment |
| CAC | lower_is_better | Monthly | GTM efficiency |
| LTV | higher_is_better | Quarterly | Customer value |
| LTV:CAC | higher_is_better | Quarterly | Unit economics health |

### Recording Values

Use `record_metric_value` to log actual values. Always record at consistent intervals. Gaps in data break trend analysis.

### Trend Analysis

Use `get_metric_trend` to see historical values. Look for:
- **Inflection points** — When did the trend change? What happened strategically?
- **Rate of change** — Is improvement accelerating or decelerating?
- **Correlation** — Do financial metrics move with strategic execution metrics?

## Reporting

### Monthly Financial Summary

A strategic financial summary covers:

1. **Cash position** — Current cash, burn rate, runway
2. **Revenue performance** — Actual vs. plan, growth rate, mix
3. **Expense analysis** — By strategy (not just by account), variance to plan
4. **Strategic alignment score** — From the latest financial scan
5. **Key financial risks** — New or escalated
6. **Outlook** — What next month looks like based on current trajectory

### Quarterly Financial Review

More comprehensive. Adds:
- Full strategy-spend alignment analysis
- Assumption validation (did financial assumptions hold?)
- Budget reforecast for next quarter
- Investment recommendations (where to increase/decrease spend)
- Fundraise readiness assessment (if applicable)

### Board-Ready Reporting

When preparing for board meetings:
1. Lead with strategic narrative, not just numbers
2. Connect financial performance to strategic milestones
3. Highlight decisions that need board input (especially funding)
4. Show runway in context of strategy timeline
5. Present risks with mitigation plans, not just risk descriptions

Use `create_document` to produce reports and store them in the workspace.

## Investor Preparation

### Financial Narrative for Fundraising

Investors want to see:

1. **Unit economics** — LTV:CAC, gross margin, payback period
2. **Growth efficiency** — Revenue growth rate vs. burn rate increase
3. **Strategic spend discipline** — Money going to the right places
4. **Runway management** — Responsible cash management
5. **Financial assumptions** — What the model depends on (and confidence levels)

### Building the Financial Model Context

Pull together:
- `get_workspace_snapshot` — Strategic context
- `get_finance_proposal` — COA structure showing strategic alignment
- `list_finance_mappings` — How spend maps to strategy
- `list_metrics` — Financial KPIs and trends
- `list_assumptions` — Financial assumptions with confidence
- `list_risks` — Financial risks

### Due Diligence Preparation

Create a document that pre-answers common investor financial questions:
- How do you allocate spend across strategic priorities?
- What are your unit economics and how are they trending?
- What assumptions underpin your financial model?
- What's your runway and when do you need to raise?
- How do you track financial alignment with strategy?

## Operational Workflows

### New Account Setup

When a new expense category emerges:
1. Determine the right account code and type
2. Place it in the COA hierarchy (under the right header)
3. Map it to the strategy it serves — immediately, not later
4. Set cost nature (fixed/variable/semi_variable/one_time)
5. Create any related metrics if the spend should be tracked as a KPI

### Budget Reallocation

When strategy priorities shift:
1. Identify accounts mapped to the deprioritised strategy
2. Review which can be reduced or eliminated
3. Identify accounts needed for the new priority
4. Create new accounts if needed, with strategy mappings
5. Document the reallocation as a decision (`create_decision`)
6. Update affected metrics and forecasts

### Month-End Close (Strategic Layer)

After the accounting close:
1. Record financial metric values (`record_metric_value`)
2. Run a quick alignment check — any new unmapped spend?
3. Update runway calculation
4. Flag any material variances to the FD
5. Update any affected assumptions if actuals diverge from plan

### Cash Flow Monitoring

Proactively monitor:
- Current runway vs. strategy timeline milestones
- Burn rate trend (increasing, stable, decreasing)
- Revenue trajectory vs. plan
- Upcoming large commitments or investments

If runway drops below strategy timeline requirements, escalate immediately.

## Working with External Systems

Stratafy's finance module connects strategic intent to financial data. The actual accounting happens elsewhere (Xero, QuickBooks, Sage, Zoho Books). The workflow:

```
Accounting System (transactions, reconciliation, compliance)
    ↓ exports data
Stratafy Finance Module (strategic alignment, mapping, scans)
    ↓ produces
Strategic Financial Intelligence (insights, decisions, risks)
```

When importing from external systems:
- Use `import_sheet_as_finance_proposal` for spreadsheet imports
- Map the external COA to Stratafy's strategic structure
- Don't try to replicate the accounting system — focus on the strategic layer

## When to Use This Knowledge

Activate this skill when:
- A user wants to set up or update financial metrics
- Someone needs a financial report or summary
- Investor preparation or board meeting prep is needed
- Month-end or quarter-end financial tasks are happening
- Budget reallocation is being discussed
- Someone asks about runway, burn rate, or cash position
- Operational finance workflows need to run
