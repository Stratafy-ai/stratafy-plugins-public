# /stratafy-cto:build-vs-buy

Structured decision framework for build-vs-buy choices, grounded in strategic context — not just cost comparison.

## Input

Describe the capability:
- What you need (e.g., "auth system", "billing integration", "analytics pipeline")
- Why now (what's driving the need)
- Any options already on the table

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "build-vs-buy"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` with `_source_plugin: "stratafy-cto"` — Company stage, team size, runway
- `get_expert_strategies` with `_source_plugin: "stratafy-cto"` — CTO's owned strategies
- `list_strategies` with `_source_plugin: "stratafy-cto"` — Which strategy does this capability serve?
- `list_initiatives` with `_source_plugin: "stratafy-cto"` — Related initiatives and their timelines
- `list_assumptions` with `_source_plugin: "stratafy-cto"` — Assumptions about build capacity, vendor reliability, etc.
- `list_risks` with `_source_plugin: "stratafy-cto"` — Risks this decision creates or mitigates
- `search_workspace` with `_source_plugin: "stratafy-cto"` with the capability description — any prior thinking or decisions

### Step 3: Apply the Framework

Evaluate across five dimensions:

**1. Strategic centrality**
- Is this capability core to your differentiation, or commodity infrastructure?
- If a competitor uses the same vendor, does that matter?
- Will this need to be deeply customised to your strategy?

**2. Time-to-value**
- How fast do you need this? What's the cost of delay?
- Build timeline vs. integration timeline — be honest about both
- What's blocked until this is in place?

**3. Total cost of ownership (3-year view)**
- Build: engineering time, maintenance, on-call, upgrades, opportunity cost
- Buy: license/usage fees, integration cost, migration risk, vendor lock-in
- Hybrid: build the core, buy the edges

**4. Team capacity**
- Do you have the expertise to build this well?
- What doesn't get built if the team builds this instead?
- Is this a hiring trigger — do you need this skill long-term?

**5. Control and risk**
- What happens if the vendor changes pricing, gets acquired, or shuts down?
- What data flows through this system? Sensitivity level?
- Can you switch later if the decision is wrong? What's the reversibility cost?

### Step 4: Score and Recommend

```
BUILD VS BUY — [Capability Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

STRATEGIC CONTEXT
  Strategy served: [strategy name]
  Initiative: [initiative name]
  Urgency: [why now — what's blocked or at risk]

ASSESSMENT
                        Build    Buy     Notes
  Strategic centrality: [H/M/L]  [H/M/L] [one line]
  Time-to-value:        [H/M/L]  [H/M/L] [one line]
  Total cost (3yr):     [H/M/L]  [H/M/L] [one line]
  Team capacity:        [H/M/L]  [H/M/L] [one line]
  Control & risk:       [H/M/L]  [H/M/L] [one line]

OPTIONS
  Option A: Build — [description]
    Pros: [list]
    Cons: [list]
    Timeline: [estimate]
    Opportunity cost: [what doesn't get built]

  Option B: Buy [vendor/tool] — [description]
    Pros: [list]
    Cons: [list]
    Timeline: [estimate]
    Annual cost: [estimate]

  Option C: Hybrid — [description, if applicable]
    [Build the core, buy the edges — or vice versa]

ASSUMPTIONS
  [What must be true for each option to work]
  [Link to workspace assumptions where relevant]

RECOMMENDATION
  [Clear recommendation with rationale]
  [Framed in strategic terms, not just cost]

REVERSIBILITY
  If this turns out to be wrong in 12 months:
  Build → Buy migration: [effort estimate]
  Buy → Build migration: [effort estimate]

NEXT STEP
  [Single concrete action to move forward]
```

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "build-vs-buy"
- `_change_reasoning`: brief explanation

## Rules

- **Stage-aware.** A seed-stage company with 12 months of runway has a different build-vs-buy calculus than a Series B company. Always factor in stage and runway.
- **Honest about build costs.** Engineers underestimate build time. Always. Add maintenance, on-call, and opportunity cost.
- **Honest about buy costs.** Vendors understate integration complexity. Always. Add customisation, migration, and lock-in risk.
- **Think in reversibility.** The best decision is often the most reversible one. If you can switch later at low cost, that changes the calculus.
- **No religion.** "We should own our stack" is a preference, not a strategy. "This capability is our core differentiator and we need full control" is a strategy.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
