# /stratafy-finance:explain-metric

Explain what a financial metric means in strategic context. Not just the number — why it matters, what drives it, and what it tells you about strategic progress.

## When to Use

- When a metric changes and you want to understand why
- When presenting metrics to non-financial stakeholders
- When a team member wants to understand a financial KPI
- When connecting a metric movement to strategic implications

## Input

Provide one of:
- **A specific metric** — "explain our burn rate"
- **A metric value** — "what does a 3.2 LTV:CAC ratio mean for us?"
- **A trend** — "our gross margin dropped from 72% to 65% — what does that mean?"
- **A comparison** — "explain why CAC went up while revenue went down"

## Process

### Step 1: Get Context
- `get_workspace_snapshot` — Company context
- `list_metrics` — Find the metric(s) in question
- `get_metric_trend` — Historical values if available
- `get_strategy_tree` — Strategic context for the metric

### Step 2: Explain the Metric

**What it is:**
Plain-language definition. No jargon without explanation.

**What ours says:**
The current value, trend, and how it compares to benchmarks or targets.

**Why it matters strategically:**
Connect the metric to specific strategies, initiatives, or objectives. A metric without strategic context is just a number.

**What drives it:**
The inputs and levers that move this metric. What would make it go up? Down?

**What to watch:**
Leading indicators, thresholds, and signals that this metric might change.

### Step 3: Contextual Recommendations

Based on the metric's current state:
- If healthy: "This validates our approach to [strategy]. Keep doing [action]."
- If concerning: "This suggests [issue] with [strategy]. Consider [action]."
- If unclear: "We need more data points. Track [additional metric] to understand the driver."

## Output

A clear, concise explanation that anyone in the organisation can understand, with strategic context that makes the number meaningful.

## Examples

**For the FD:**
> "Burn rate hit R380K this month, up 12% from R340K. The increase is split: R25K from the new engineering hire (mapped to Product Architecture — strategic) and R15K from a SaaS tool expansion (unmapped — investigate). Net: 65% of the increase is strategic, 35% needs justification."

**For the Finance Team:**
> "Burn rate is how much cash we spend each month. Ours is R380K — meaning at our current cash balance, we have about 14 months before we run out (that's called 'runway'). This month it went up because we hired a new engineer. That's a planned investment in our Product strategy, so it's expected. But there's also R15K in new SaaS tools that nobody mapped to a strategy yet — can you help me figure out what those are for?"
