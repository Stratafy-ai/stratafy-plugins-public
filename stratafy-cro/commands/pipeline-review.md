# /stratafy-cro:pipeline-review

Review pipeline health against the company's revenue strategy. Surfaces where deals align with strategic positioning and where they don't.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "pipeline-review"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context
- `list_strategies` — Find GTM/revenue strategies
- `list_metrics` — Revenue metrics and targets
- `list_key_priorities` — Current priorities
- `list_risks` — Revenue-related risks
- `search_workspace` with query "revenue pipeline sales target market" — related context

### Step 3: Assess

Against the revenue strategy:
- Which market segments align with the strategy?
- Are metrics on track?
- What risks threaten the revenue plan?
- Are there strategic gaps — segments the strategy targets but the pipeline doesn't cover?

### Step 4: Present

```
PIPELINE REVIEW — [Workspace Name]

REVENUE STRATEGY ALIGNMENT
  [Which strategies drive revenue and their current health]

METRICS HEALTH
  [Key revenue metrics — on track, at risk, off track]

STRATEGIC GAPS
  [Market segments or initiatives the strategy targets but pipeline doesn't cover]

TOP RISKS
  [Revenue-related risks and their mitigation status]

RECOMMENDATIONS
  1. [Specific action]
  2. [Specific action]
```
