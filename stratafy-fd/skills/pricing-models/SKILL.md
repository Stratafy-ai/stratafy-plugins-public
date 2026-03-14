# Pricing Models

You help users design, analyse, and evolve pricing strategies that connect revenue architecture to strategic intent. Pricing isn't just a number — it's the mechanism that turns value creation into value capture. Every pricing decision should trace back to strategy.

## The Core Question

> Does our pricing capture the value our strategy creates — for the right customers, at the right time, in the right way?

If a company builds a premium product but prices like a commodity, value leaks. If they price for enterprise but sell to startups, the model creates friction instead of revenue. Making this visible — and fixable — is the highest-value pricing activity.

## Pricing Model Types

### Common SaaS Pricing Models

| Model | How It Works | Best When | Watch Out For |
|---|---|---|---|
| **Flat rate** | Single price, all features | Simple product, single persona | Leaves money on table as you grow |
| **Per seat** | Price × number of users | Value scales with team size | Discourages adoption within orgs |
| **Usage-based** | Pay for what you consume | Value correlates with usage | Revenue volatility, hard to forecast |
| **Tiered** | Good/Better/Best packages | Multiple customer segments | Tier boundaries create friction |
| **Freemium** | Free tier + paid upgrades | Large addressable market, low marginal cost | Conversion rate is everything |
| **Value-based** | Priced on outcome delivered | Measurable customer ROI | Requires proving value attribution |
| **Hybrid** | Combination of above | Complex value delivery | Complexity in communication |

### Pricing Dimensions

Every pricing model has levers to pull:

- **Base price** — The anchor point. What does the customer see first?
- **Unit of value** — What are they paying per? (seat, API call, workspace, GB, outcome)
- **Packaging** — How are features grouped? What's in each tier?
- **Discounting** — Annual vs. monthly, volume breaks, startup programs
- **Expansion mechanics** — How does spend grow within an account? (more seats, more usage, tier upgrade)
- **Currency & localisation** — Pricing for different markets

## Designing a Pricing Model

### Step 1: Strategic Foundation

Before touching numbers, understand:

1. **Who are we pricing for?** — Pull customer segments from strategies and initiatives
2. **What value do we create?** — Map features/capabilities to customer outcomes
3. **What's our strategic position?** — Premium, competitive, penetration, value?
4. **What does the strategy tree say?** — Revenue targets, market segments, competitive positioning

### Step 2: Value Mapping

Connect product capabilities to customer value:

```
Feature/Capability → Customer Outcome → Willingness to Pay
─────────────────────────────────────────────────────────
AI strategy tools  → Faster decisions    → High (saves exec time)
Collaboration      → Team alignment      → Medium (nice-to-have)
Reporting          → Board confidence    → High (reduces risk)
API/Integrations   → Workflow efficiency → Variable (depends on stack)
```

### Step 3: Model Architecture

Design the pricing structure:

**Packaging (tiers/plans):**
- What's in each tier?
- What's the upgrade trigger? (the feature or limit that forces tier change)
- Is there a free tier? What's its purpose? (lead gen, PLG, market penetration)

**Pricing levels:**
- What's the entry price? (must clear the "worth my time to evaluate" bar)
- What's the expansion ceiling? (how much can a single account grow to?)
- What's the average expected revenue per account?

**Unit economics:**
- At each price point, what's the gross margin?
- What's the cost to serve this tier?
- What's the CAC payback period at this price?

### Step 4: Competitive Context

Position pricing relative to alternatives:

- **Direct competitors** — What do they charge? What's their model?
- **Indirect competitors** — What are customers spending on the job-to-be-done today? (consultants, spreadsheets, other tools)
- **Do nothing** — What's the cost of the status quo?

### Step 5: Scenario Modelling

Model revenue under different assumptions:

```
SCENARIO: Tiered SaaS (Starter/Pro/Enterprise)

Assumptions:
  Starter: R500/mo, 60% of customers
  Pro: R2,000/mo, 30% of customers
  Enterprise: R8,000/mo, 10% of customers
  Monthly churn: 3%
  New customers/month: 5 → 8 → 12 (Q1→Q2→Q3)

Revenue forecast:
  Month 3:  R18K MRR (15 customers)
  Month 6:  R52K MRR (38 customers after churn)
  Month 12: R142K MRR (89 customers after churn)

Sensitivity:
  If avg price +20%: Month 12 → R170K MRR
  If churn halved:   Month 12 → R168K MRR
  If growth +50%:    Month 12 → R198K MRR
```

## Analysing an Existing Pricing Model

### Health Check

Pull workspace data and assess:

1. **Revenue concentration** — Is revenue spread across tiers or concentrated in one?
2. **Expansion rate** — Are customers upgrading? What's the net revenue retention?
3. **Churn by tier** — Which tier has highest churn? (often reveals a value gap)
4. **Conversion funnel** — Free → paid, trial → paid, demo → paid rates
5. **Competitive position** — Are we priced above, below, or at market?
6. **Margin by tier** — Does every tier make money after cost-to-serve?

### Common Pricing Problems

| Symptom | Likely Cause | Investigation |
|---|---|---|
| High churn on cheapest tier | Value doesn't justify price, or wrong customers | Check if tier delivers enough value to retain |
| Nobody buys top tier | Tier gap too large, or features don't justify premium | Review tier boundaries and upgrade triggers |
| Customers ask for custom pricing | Tiers don't match real segments | Analyse what customers actually need vs. what tiers offer |
| Revenue grows slower than customers | Pricing doesn't capture expansion value | Check expansion mechanics and upgrade triggers |
| Win rate drops after pricing conversation | Price-value perception mismatch | Review how pricing is presented and justified |
| High discount rates to close deals | List price disconnected from willingness to pay | Analyse discount patterns — systematic or ad-hoc? |

### Strategic Alignment Check

Connect pricing back to strategy:

- Does pricing **support** the GTM strategy? (penetration pricing for land-and-expand, premium for enterprise-first)
- Does pricing **reflect** the product strategy? (AI features in premium tier if AI is the differentiator)
- Does pricing **enable** the growth strategy? (expansion mechanics that grow accounts organically)
- Does pricing **fund** the strategy timeline? (revenue ramp fast enough for runway)

## Documenting Pricing Models

Store pricing models as Stratafy documents with structured sections:

### Pricing Model Document Structure

```
1. Strategic Context — Why this model, connected to which strategies
2. Target Segments — Who each tier/plan serves
3. Value Map — Features → outcomes → willingness to pay
4. Pricing Architecture — Tiers, prices, packaging
5. Unit Economics — Margins, CAC payback, LTV projections per tier
6. Competitive Position — Where we sit relative to alternatives
7. Assumptions — What this model depends on being true
8. Scenarios — Revenue projections under different conditions
9. Review Triggers — When to revisit this model
```

### Connecting to the Workspace

When creating or updating a pricing model:

- `create_document` — Store the pricing model as a workspace document
- `create_assumption` — Log pricing assumptions (willingness to pay, conversion rates, churn rates)
- `link_assumption_to_context` — Connect pricing assumptions to revenue strategies
- `create_risk` — Log pricing risks (competitive pressure, market shift, value perception)
- `create_metric` — Set up pricing KPIs (ARPU, conversion rate, expansion revenue, discount rate)
- `create_decision` — Document pricing decisions with rationale and alternatives considered
- `create_insight` — Capture pricing discoveries from data or customer feedback

## Pricing Metrics

### Key Pricing KPIs

| Metric | What It Tells You | Polarity |
|---|---|---|
| **ARPU** (Avg Revenue Per User/Account) | Overall pricing effectiveness | higher_is_better |
| **Conversion rate** (free→paid or trial→paid) | Entry price/value balance | higher_is_better |
| **Expansion revenue %** | Pricing captures growing value | higher_is_better |
| **Net Revenue Retention** | Pricing + value = sticky revenue | higher_is_better |
| **Discount rate** | Pricing credibility | lower_is_better |
| **Time to first value** | Pricing friction vs. activation | lower_is_better |
| **Revenue per tier** | Tier design effectiveness | balanced |
| **Upgrade rate** | Tier boundary design | higher_is_better |

### Setting Up Pricing Metrics

Use `create_metric` with appropriate thresholds:

```
ARPU:
  green_threshold: target ARPU (e.g., R2,000)
  yellow_threshold: 80% of target
  red_threshold: 60% of target
  polarity: higher_is_better

Net Revenue Retention:
  green_threshold: 110% (best-in-class SaaS)
  yellow_threshold: 100% (break-even on existing customers)
  red_threshold: 90% (net contraction)
  polarity: higher_is_better
```

## Pricing Reviews

### When to Review Pricing

- **Quarterly** — Check metrics, compare to plan, adjust if needed
- **After major product launch** — New value created may justify new pricing
- **Competitive shift** — Major competitor changes pricing
- **Segment shift** — Customer mix changing (moving upmarket or downmarket)
- **Strategy change** — New strategic direction may need different pricing
- **Revenue miss** — Pricing may be the bottleneck

### Review Process

1. Pull all pricing metrics and trends
2. Analyse by segment and tier
3. Compare to competitive landscape
4. Check assumption validity
5. Propose adjustments with modelled impact
6. Document decision

## When to Use This Knowledge

Activate this skill when:
- A user wants to design pricing for a new product or feature
- Someone asks about pricing strategy, tiers, or packaging
- Revenue growth is underperforming and pricing may be the lever
- A pricing decision needs to be made (new tier, price change, discount policy)
- Competitive pricing intelligence needs interpretation
- The conversation involves ARPU, conversion rates, expansion revenue, or pricing metrics
- Someone asks "how should we price this?" or "is our pricing right?"
- Fundraising prep needs a pricing/revenue model narrative
