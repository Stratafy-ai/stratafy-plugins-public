---
name: health-check
description: Run a strategy health check across the workspace
---

# /stratafy:health-check

Run a strategy health check across the workspace. This command surfaces the health scores produced by Stratafy's automated scoring system — structural completeness, content quality, measurement coverage, relationship depth, and staleness — and presents them as an actionable diagnostic.

Unlike `/stratafy:pulse` (which covers execution and risk broadly) or `/stratafy:strategy-review` (which is a deep qualitative analysis), this command focuses specifically on the **health scoring system**: what the automated checks found, where scores are dropping, and what concrete actions would improve them.

## When to Use

- After distillation runs — see what the health scorer found
- Checking which strategies need structural attention
- Before a strategy review session — fix health issues first so the review is meaningful
- Monitoring health trends over time
- When onboarding a new client — quick assessment of strategy quality
- As a regular cadence check (weekly/fortnightly)

## Process

### Step 1: Load Health Scores

Pull the current health state for all strategies:

- `list_strategies` with `include_closed: false` — returns all open strategies with health fields (`health_score`, `health_status`, `health_alerts`, `health_scored_at`)

### Step 2: Triage by Status

Group strategies by health status and sort by score within each group:

1. **Red** (score < 50) — strategies that need immediate attention
2. **Amber** (score 50-74) — strategies with issues worth addressing
3. **Green** (score >= 75) — healthy strategies

Note any strategies that have never been scored (`health_score` is null) — these need a distillation run or manual review.

### Step 3: Analyse Alert Patterns

For each strategy, the `health_alerts` array contains individual check results across 5 categories:

**Structural (max 20 points)**
- Missing strategy type, time horizon, tagline, lead, description
- Description too long (content belongs in content field)
- Non-company strategy without parent

**Content (max 20 points)**
- No content body at all
- Content too thin (below word count threshold)
- LLM-assessed flags: task list not strategy, no theory of change, no constraints, vague constraints, no assumptions stated, description-is-content, incoherent structure

**Measurement (max 25 points)**
- No active initiatives
- Low or zero objective coverage across initiatives
- No metrics linked
- Objectives without target values

**Relationship (max 15 points)**
- Initiative sprawl (too many active initiatives)
- Initiative type mismatch (operational under strategic)
- No risks identified
- No assumptions tracked

**Staleness (max 20 points)**
- Strategy not updated within threshold (adjusted by time horizon)
- Overdue initiatives past target completion date
- Stale objectives past target date with unchanged values
- Unvalidated critical assumptions (90+ days)

Look for cross-cutting patterns:
- Are most red strategies in one branch? That branch needs focused attention.
- Is staleness the dominant issue? The team may not be reviewing strategies regularly.
- Are measurement scores consistently low? The workspace needs better objective/metric discipline.
- Are content scores low across the board? Strategies need deeper articulation.

### Step 4: Present the Health Check

```
STRATEGY HEALTH CHECK — [Workspace Name]
Date: [today]

WORKSPACE HEALTH OVERVIEW
━━━━━━━━━━━━━━━━━━━━━━━━
Overall: X strategies scored | Average score: XX/100
  🔴 Red:    X strategies (need immediate attention)
  🟡 Amber:  X strategies (issues worth addressing)
  🟢 Green:  X strategies (healthy)
  ⚪ Unscored: X strategies (need distillation)

DOMINANT ISSUES
━━━━━━━━━━━━━━
[1-3 patterns observed across strategies]
e.g., "Staleness is the biggest factor — 4/6 strategies haven't been updated in 30+ days"
e.g., "Measurement coverage is weak — most strategies lack linked metrics"

━━━ RED STRATEGIES (Immediate Attention) ━━━

[Strategy Name] — Score: XX/100
  Category breakdown: Structural X/20 | Content X/20 | Measurement X/25 | Relationship X/15 | Staleness X/20
  Top alerts:
  • [alert message] (−X pts)
  • [alert message] (−X pts)
  • [alert message] (−X pts)
  Fix priority: [1-2 sentence recommendation]

━━━ AMBER STRATEGIES (Worth Addressing) ━━━

[Strategy Name] — Score: XX/100
  Category breakdown: ...
  Top alerts:
  • ...
  Fix priority: ...

━━━ GREEN STRATEGIES (Healthy) ━━━

[Strategy Name] — Score: XX/100
  [Brief note on any minor alerts, or "No issues detected"]

━━━ RECOMMENDED ACTIONS ━━━

Priority fixes (biggest score impact):
1. [Action] — would improve [strategy] score by ~X points
2. [Action] — would improve [strategy] score by ~X points
3. [Action] — would improve [strategy] score by ~X points

Quick wins (can be done now):
- [e.g., "Set time horizon on Strategy X (+3 points)"]
- [e.g., "Assign a lead to Strategy Y (+4 points)"]
- [e.g., "Add target dates to 3 initiatives"]
```

### Step 5: Offer Next Steps

After presenting the health check, offer actionable follow-ups:

- "Want me to fix the quick wins? I can update the fields that are easy to resolve."
- "Should I run `get_structure_diagnostics` on the red strategies for a deeper structural analysis?"
- "Want me to do a full `/stratafy:strategy-review` on the worst-scoring strategy?"
- "Should I trigger a distillation for the unscored strategies?" (if any)

## Calculating Category Scores

When presenting category breakdowns, calculate per-category scores from alerts:

```
Category score = Category max − min(sum of deductions in category, category cap)
```

Where category caps are: Structural=20, Content=20, Measurement=25, Relationship=15, Staleness=20.

For example, if a strategy has alerts with 8 + 5 = 13 points deducted in Measurement:
- Measurement score = 25 − 13 = 12/25

## Understanding Score Thresholds

- **90-100**: Exceptionally well-defined strategy — rare and aspirational
- **75-89**: Healthy — structurally sound with minor gaps
- **50-74**: Needs work — specific categories are dragging the score down
- **25-49**: Significant issues — multiple categories failing
- **0-24**: Critical — strategy needs fundamental rework

## What This Command Does NOT Do

- **Deep qualitative review** — use `/stratafy:strategy-review` for that
- **Structural tree analysis** — use `/stratafy:strategy-tree-review` for architecture checks
- **Execution tracking** — use `/stratafy:pulse` for OKR/initiative progress
- **Trigger re-scoring** — health scores are computed by the distillation pipeline and cron sweeps
- **Risk/signal analysis** — health checks focus on strategy quality, not environmental factors

## For Coaches

When running health checks across client workspaces:
- Start each session by running health check to see what's changed since last time
- Track score trends: "Last session you were at 62, now you're at 71 — good progress on measurement"
- Use red/amber strategies as the agenda for the coaching session
- Focus on one category at a time — trying to fix everything at once overwhelms
- Celebrate green strategies — positive reinforcement matters
