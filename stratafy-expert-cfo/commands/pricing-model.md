# /stratafy-fd:pricing-model

Design or analyse a pricing model — connecting revenue architecture to strategic intent. Not "what should we charge?" in isolation, but "what pricing structure turns our strategy into sustainable revenue?"

## When to Use

- Designing pricing for a new product, feature, or market
- Reviewing whether current pricing captures the value being created
- Preparing pricing narrative for investor conversations
- After a strategy pivot that changes the value proposition
- When revenue growth lags customer growth (pricing may be the bottleneck)
- Competitive shift that forces pricing reconsideration

## Input

Provide one of:
- **Nothing** — Full pricing landscape analysis for the workspace
- **"Create a pricing model"** — Design mode: build a new model from strategy context
- **"Analyse our pricing"** — Analysis mode: evaluate current pricing health
- **A specific question** — "should we add an enterprise tier?" or "is freemium right for us?"
- **A scenario** — "what if we increase prices 20%?" or "model usage-based pricing"

## Process

### Design Mode (creating a new pricing model)

#### Step 1: Gather Strategic Context

Everything needed in parallel:
- `get_workspace_snapshot` — Company stage, positioning, context
- `get_strategy_tree` — Full strategy hierarchy, priorities, GTM approach
- `list_strategies` — Revenue-related strategies and initiatives
- `list_metrics` — Existing revenue/pricing metrics
- `list_assumptions` — Revenue and market assumptions
- `list_objectives` — Revenue targets and growth objectives

#### Step 2: Define the Value Architecture

Map what the product does to what customers pay for:

```
VALUE MAP — [Product Name]

Segment: [Target customer]
Job to be done: [What they're hiring the product for]

Core Value Drivers:
  1. [Capability] → [Outcome] → [Willingness to pay: High/Med/Low]
  2. [Capability] → [Outcome] → [Willingness to pay: High/Med/Low]
  3. [Capability] → [Outcome] → [Willingness to pay: High/Med/Low]

Expansion triggers:
  - More [users/usage/data/features] as the customer grows
  - Moving from [basic need] to [strategic need]
```

#### Step 3: Design the Model

Propose a pricing architecture with rationale:

```
PRICING MODEL — [Product Name]
Model type: [Tiered / Usage-based / Hybrid / etc.]
Strategic rationale: [Why this model fits the strategy]

┌─────────────┬──────────────┬──────────────┬──────────────┐
│             │ Starter      │ Pro          │ Enterprise   │
├─────────────┼──────────────┼──────────────┼──────────────┤
│ Price       │ R500/mo      │ R2,000/mo    │ R8,000/mo    │
│ Target      │ Small teams  │ Growing cos  │ Established  │
│ Seats       │ Up to 3      │ Up to 15     │ Unlimited    │
│ Core value  │ [feature]    │ [feature]    │ [feature]    │
│ Upgrade     │ Hits seat    │ Needs [X]    │ Custom       │
│ trigger     │ limit        │ feature      │ requirements │
│ Margin      │ ~80%         │ ~75%         │ ~70%         │
├─────────────┼──────────────┼──────────────┼──────────────┤
│ Strategic   │ Land. Build  │ Expand.      │ Lock in.     │
│ purpose     │ habit.       │ Prove ROI.   │ Strategic    │
│             │ Generate     │ Fund the     │ partnership. │
│             │ case studies │ engine       │ Reference    │
└─────────────┴──────────────┴──────────────┴──────────────┘
```

#### Step 4: Model the Revenue Scenarios

Run 3 scenarios (conservative, base, optimistic):

```
REVENUE SCENARIOS — Base Case

Assumptions:
  Customer mix: 60% Starter / 30% Pro / 10% Enterprise
  New customers/mo: 5 → 8 → 12 (over 12 months)
  Monthly churn: 3% Starter / 1.5% Pro / 0.5% Enterprise
  Annual discount: 20% (assume 30% take annual)

Projected MRR:
  Month 3:   R[X]  ([Y] customers)
  Month 6:   R[X]  ([Y] customers)
  Month 12:  R[X]  ([Y] customers)
  Month 24:  R[X]  ([Y] customers)

Unit Economics:
  Blended ARPU:         R[X]/mo
  Blended CAC payback:  [X] months
  Projected LTV:        R[X]
  LTV:CAC ratio:        [X]:1
```

#### Step 5: Log Everything in the Workspace

- `create_document` — Full pricing model document
- `create_assumption` — Each pricing assumption (willingness to pay, conversion rate, churn by tier, customer mix)
- `link_assumption_to_context` — Connect to revenue strategies
- `create_metric` — ARPU, conversion rate, expansion revenue, discount rate, revenue by tier
- `create_risk` — Pricing risks (competitive undercut, value perception, market shift)
- `create_decision` — The pricing decision itself with rationale and alternatives considered

### Provenance Context
For every mutation in this step, include:
- `_source_plugin`: "stratafy-fd"
- `_source_command`: "pricing-model"
- `_change_reasoning`: Brief explanation (e.g. "Logging pricing model assumptions — 3-tier SaaS structure based on strategy analysis")

### Analysis Mode (evaluating existing pricing)

#### Step 1: Gather Current State

Everything needed in parallel:
- `get_workspace_snapshot` — Context and stage
- `list_metrics` — Revenue and pricing metrics with current values
- `get_metric_trend` — Trends for ARPU, conversion, churn, expansion
- `list_strategies` — Current strategy priorities
- `list_assumptions` — Pricing-related assumptions
- `search_documents` — Existing pricing model documents

#### Step 2: Pricing Health Dashboard

```
PRICING HEALTH — [Workspace Name]
As at: [date]

MODEL: [Current model type]

REVENUE METRICS
  ARPU:                    R[X]/mo    [target]  🟢/🟡/🔴
  Net Revenue Retention:   [X]%       [target]  🟢/🟡/🔴
  Expansion Revenue:       [X]% of total        🟢/🟡/🔴
  Discount Rate:           [X]% avg              🟢/🟡/🔴

CONVERSION & GROWTH
  Trial → Paid:            [X]%       [target]  🟢/🟡/🔴
  Free → Paid:             [X]%       [target]  🟢/🟡/🔴
  Upgrade Rate:            [X]%/mo    [target]  🟢/🟡/🔴

UNIT ECONOMICS
  CAC Payback:             [X] months            🟢/🟡/🔴
  Gross Margin:            [X]%                  🟢/🟡/🔴
  LTV:CAC:                 [X]:1                 🟢/🟡/🔴

TIER DISTRIBUTION
  [Tier 1]:  [X]% of customers, [Y]% of revenue
  [Tier 2]:  [X]% of customers, [Y]% of revenue
  [Tier 3]:  [X]% of customers, [Y]% of revenue
```

#### Step 3: Diagnosis

Identify pricing problems and opportunities:

**Value Capture:** Is the pricing model capturing the value being created?
- Revenue per customer vs. value delivered
- Features customers use most vs. what they pay for
- Expansion triggers firing or not

**Strategic Fit:** Does pricing support or hinder the strategy?
- GTM friction (pricing too complex for self-serve, too cheap for enterprise sales)
- Segment mismatch (pricing attracts wrong customers)
- Competitive vulnerability (easily undercut or outpositioned)

**Assumption Validity:** Are the pricing assumptions still holding?
- Willingness to pay — confirmed by conversion rates?
- Customer mix — matching the model?
- Churn rates — as expected by tier?

#### Step 4: Strategic Narrative

Synthesise findings into a coherent assessment:

> "Current pricing is optimised for land but not expand. The R500 Starter tier converts well (8% trial-to-paid) but customers plateau — only 12% upgrade to Pro within 6 months. The upgrade trigger (seat limits) doesn't match how customers grow into the product. Consider switching the trigger to a value-based limit (e.g., number of active strategies) that naturally increases as customers embed the product in their workflow."

#### Step 5: Recommendations

Provide 3-5 specific, actionable recommendations:

- **Adjust:** "[Tier/price/packaging change] — [why] — [modelled impact]"
- **Add:** "New tier/plan for [segment] — [gap it fills] — [revenue potential]"
- **Remove:** "Eliminate [tier/discount/feature gate] — [why it's hurting]"
- **Test:** "A/B test [pricing change] — [hypothesis] — [success metric]"
- **Track:** "Add metric [X] — [why it's missing from pricing visibility]"
- **Investigate:** "[Signal] suggests [issue] — dig into [specific data]"

Offer to execute: update metrics, create assumptions, log decisions, produce documents.

## Output

**Design mode:** A complete pricing model document with strategic rationale, tier architecture, revenue scenarios, unit economics, and workspace artefacts (assumptions, metrics, risks, decisions) all logged.

**Analysis mode:** A pricing health assessment with dashboard, diagnosis, strategic narrative, and concrete recommendations. End with next steps the user can approve.

## Audience Adaptation

**For the FD/CFO:**
> "Three pricing issues: (1) ARPU is flat at R1,200 while product value has increased 40% since launch — we're leaving money on the table. (2) Enterprise tier has zero customers after 6 months — either the tier is wrong or the sales motion doesn't exist. (3) 35% discount rate on Pro tier suggests list price has no credibility. Recommend: raise Starter 15%, redesign Enterprise as a custom-priced tier, implement discount governance."

**For the founder/CEO:**
> "Your pricing tells investors a story: R1,200 ARPU with 8% conversion says 'we have product-market fit at the small end.' But no enterprise customers says 'we haven't validated upmarket yet.' Before fundraising, you need either (a) 2-3 enterprise customers proving premium pricing, or (b) a compelling narrative for why you're choosing to stay downmarket. Either is fine — but 'we have an enterprise tier nobody buys' is neither."

**For the team:**
> "Our pricing has three tiers: Starter at R500 for small teams getting started, Pro at R2,000 for growing companies, and Enterprise at R8,000 for large organisations. Most customers are on Starter — that's by design, because we want people to try the product easily. The goal is to make the product so valuable that teams naturally grow into Pro. Right now about 12% upgrade — we'd like that to be 25%."
