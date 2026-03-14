# /stratafy-fd:analyse-metrics

Analyse the full metrics landscape — health, gaps, story. Not "what does this metric mean?" but "are we measuring the right things, are those things healthy, and what's the strategic narrative?"

## When to Use

- Regular check-in on metrics health ("how are our metrics looking?")
- Preparing for investor update, board meeting, or strategy review
- After adding new strategies or initiatives — to check if measurement followed
- When something feels off but you can't pinpoint which metric to look at
- When a new team member or advisor needs the full metrics picture

## Input

Provide one of:
- **Nothing** — Full landscape analysis across all metrics
- **A specific strategy** — "analyse metrics for our GTM strategy"
- **A category** — "analyse our pipeline metrics"
- **A single metric** — "analyse our conversion rate" (falls back to deep-dive mode)
- **A comparison** — "why is CAC rising while pipeline is growing?"

## Process

### Step 1: Gather Context

Everything needed in parallel:
- `get_workspace_snapshot` — Company context, stage, strategies
- `list_metrics` — All metrics with current values, targets, thresholds
- `list_strategies` — Active strategies (to check for measurement gaps)
- `list_initiatives` — Active initiatives (same)
- `list_objectives` — Objectives with targets (natural metric anchors)

For individual metric deep-dives, also pull:
- `get_metric_trend` — Historical values
- `get_metric` — Full detail including linked strategies

### Step 2: Health Check (the traffic lights)

For each metric, determine status:
- 🟢 **On track** — At or above green threshold, trending well
- 🟡 **Watch** — Between yellow and green, or trending in wrong direction
- 🔴 **Action needed** — Below red threshold, or stalled at zero with time pressure
- ⚪ **No data** — No current value recorded, or stale (not measured recently)

Group by strategic theme, not just category. A CFO thinks in terms of "what's the funding picture" (combines multiple categories), not isolated categories.

Present as a compact dashboard:

```
METRICS HEALTH — [Workspace Name]
As at: [date]

DEMAND & PIPELINE (3 🟢, 1 🟡, 0 🔴)
  Lighthouse Pipeline Size:     11/20  🟡  Pipeline growing but needs 2x for seed window
  Demos Completed:               5/10  🟢  On pace — exceeded original target
  Pipeline-to-Paid Conversion:   9%/20% 🟡  Core bottleneck — investigate why
  Active Lighthouse Customers:   1/3   🟡  Most critical gap for PMF narrative

REVENUE & RETENTION (2 metrics)
  MRR (ZAR):                    R25K/R100K  🟡  First revenue — need 3 more customers
  Customer Retention Rate:      100%/80%    🟢  Perfect but sample size = 1

FUNDING READINESS (3 metrics)
  Qualified Investor Meetings:   0/10  🔴  Fundraising engine not started
  Investors in Active Diligence: 0/3   🔴  Blocked by upstream (no meetings)
  Seed Round Readiness Score:    52/80 🟡  Evidence gap holding score down
```

### Step 3: Gap Analysis (what's missing?)

This is the differentiator:

1. **Strategies without metrics** — List every active strategy and check if at least one metric measures its progress. Flag unmonitored strategies.
2. **Initiatives without metrics** — Same for initiatives. If an initiative is "in progress" but nothing measures whether it's working, that's a blind spot.
3. **Objectives without metrics** — Objectives with targets but no metric tracking actual values.
4. **Missing stage-appropriate metrics** — Based on company stage and strategy, identify metrics the company *should* have but doesn't. For a pre-seed SaaS: burn rate, runway, CAC, unit economics, NPS/CSAT.
5. **Stale metrics** — Metrics that exist but haven't been measured recently. A metric nobody updates is worse than no metric — it creates false confidence.
6. **Vanity metrics** — Metrics that look good but don't drive decisions. Flag any metric where the description itself says "not a strategic metric" (these exist — the workspace has several).

Present as:

```
MEASUREMENT GAPS

Unmonitored strategies (7 of 27 strategies have no metric):
  - Partner Plugin Ecosystem → No metric for partner adoption or channel revenue
  - Category Creation → No metric for category awareness or share of voice
  - AI Leverage & Automation → No metric for automation coverage or time saved
  ...

Missing metrics for this stage:
  - Burn Rate (monthly cash outflow) — Critical for runway calculation
  - CAC (customer acquisition cost) — Needed before seed conversations
  - Runway (months of cash remaining) — Every investor will ask

Vanity metrics to reconsider:
  - Weekly Dev Velocity (167 commits) — Self-described as "not strategic"
  - MCP Tools Distinct (169) — Impressive number but doesn't indicate value
  - Glossary Terms (48/50) — Complete, no longer needs tracking
```

### Step 4: Strategic Narrative (the story the metrics tell)

Synthesise the metrics into a coherent 3-4 paragraph narrative. This is what goes into an investor update, a board memo, or a strategy review document. Not metric-by-metric recital.

Structure:
1. **Where we are** — One sentence positioning statement based on metrics
2. **What's working** — Evidence from green/improving metrics
3. **Where the friction is** — Evidence from red/yellow metrics
4. **What needs to happen** — The critical path implied by the metrics

Example tone:
> "Stratafy has architectural depth (15 domain systems, 169 MCP tools) and early revenue validation (R25K MRR, 100% retention) but faces a conversion bottleneck — only 9% of pipeline converts to paid. The critical path is clear: convert 2-3 more of the 11 qualified prospects to demonstrate PMF, then use those proof points to open investor meetings (currently zero). The content engine is producing (29 articles) but not yet generating organic traffic, meaning growth depends entirely on outbound — which doesn't scale and won't satisfy seed investors."

### Step 5: Recommendations

Based on the analysis, provide 3-5 specific recommendations:

- **Add metric:** "[Metric name] — [why it matters] — [targets and thresholds]"
- **Retire metric:** "[Metric name] — [why it's no longer useful]"
- **Investigate:** "[Metric name] is showing [pattern] — dig into [specific thing]"
- **Reclassify:** "[Metric name] should move from [category] to [category]" or "mark as key metric"
- **Set target:** "[Metric name] has no target — suggest [value] based on [benchmark/strategy]"

Offer to execute recommendations directly: create new metrics, update existing ones, retire vanity metrics.

## Output

A structured analysis that any stakeholder can read, with three clear sections: how things look right now (health), what we're not measuring (gaps), and what it all means (narrative). End with concrete next steps.

## Single Metric Deep-Dive (fallback mode)

When the user asks about a specific metric (e.g., "analyse our conversion rate"), switch to deep-dive mode:

**Current State:** Value, target, threshold status, trend direction
**Strategic Connection:** Which strategies/initiatives this metric serves. A metric without a strategic home is just a number.
**What Drives It:** The inputs and levers. What makes it go up or down?
**What It's Telling Us:** Interpretation — not "conversion is 9%" but "9% conversion with 11 prospects means ~1 new customer per quarter at current rates, which won't hit the 3-customer target by June"
**What to Watch:** Leading indicators, related metrics, and thresholds that signal change
**Recommendation:** One clear action based on the metric's current state

## Audience Adaptation

**For the FD/CFO:**
> "19 metrics tracked, 10 marked as key. Seven of 27 strategies have zero measurement. Pipeline-to-paid at 9% is the constraint — everything upstream looks healthy but we're leaking at conversion. Three metrics should be retired (vanity), three should be added (burn rate, CAC, runway). Net: you're measuring product well but flying blind on unit economics."

**For the founder/CEO:**
> "Your metrics tell a story of a product that works but a GTM that hasn't found its rhythm yet. The numbers you need for investor conversations — burn rate, CAC, runway — don't exist in the system yet. The good news: your pipeline and retention metrics are healthy foundations. The gap is converting interest into revenue, and measuring the cost of doing so."

**For the team:**
> "Here's our scorecard. Green means we're on track, yellow means keep an eye on it, red means we need to act. Right now we're green on product and content, yellow on pipeline and revenue, and red on funding activity. The most important thing any of us can do this month is help convert qualified prospects into paying customers."
