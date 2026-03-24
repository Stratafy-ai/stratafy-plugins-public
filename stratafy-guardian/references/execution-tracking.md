---
name: execution-tracking
description: Monitor and manage strategic execution including OKRs, initiatives, metrics, assumptions, and risks
---

# Execution Tracking

You help users monitor and manage strategic execution — the bridge between strategy and results. This covers OKRs, initiatives, metrics, assumptions, and risks.

## OKR Management

### Creating Objectives
Use `create_objective` to set objectives. Good objectives are:
- **Qualitative and aspirational** — "Establish market leadership in mid-market SaaS"
- **Time-bound** — linked to a quarter or year
- **Connected** — attached to a strategy or initiative via `strategy_id`

### Key Results
Each objective should have 2-5 key results. Key results are:
- **Quantitative and measurable** — "Increase ARR from $1M to $2M"
- **Binary-verifiable** — at the end of the period, it either happened or didn't
- **Ambitious but achievable** — 70% completion is a good outcome for stretch goals

### Scoring
Use `update_okr_score` to update progress. Scoring patterns:
- **0.0-0.3** — Off track, needs intervention
- **0.4-0.6** — Making progress, may need adjustment
- **0.7-1.0** — On track or completed
- Score at regular intervals (weekly or bi-weekly) to maintain momentum

## Initiative Tracking

### Initiative Health
When reviewing initiatives, check:
1. **Status** — is it still `active` or `in_progress`? Should it be `completed` or `cancelled`?
2. **Timeline** — has the `target_completion_date` passed? Is `start_date` realistic?
3. **Priority alignment** — does the priority (low/medium/high/critical) still reflect reality?
4. **Strategy connection** — is the parent strategy still active and relevant?

### Updating Initiatives
Use `update_initiative` to:
- Change status as work progresses
- Adjust timelines when reality shifts
- Update descriptions with current context
- Reassign leads when ownership changes

## Assumption Management

Assumptions are the hidden foundation of every strategy. Managing them is where strategic discipline lives.

### The Assumption Lifecycle
```
Create (hypothesis) → Validate/Invalidate → Update confidence → Link to strategies
```

### Confidence Levels
- **hypothesis** — We believe this but haven't tested it
- **likely** — Early evidence supports this
- **validated** — Strong evidence confirms this
- **invalidated** — Evidence disproves this

### Impact Assessment
Rate `impact_if_wrong` for every assumption:
- **critical** — Strategy fails if this assumption is wrong
- **high** — Major strategic pivot needed
- **medium** — Adjustment required but strategy survives
- **low** — Minor course correction

### Linking Assumptions
Use `link_assumption_to_context` to connect assumptions to the strategies they underpin. When an assumption is invalidated, every linked strategy needs review.

## Risk Management

### Risk Assessment
Every risk needs:
- **likelihood** (low/medium/high) — how probable is this?
- **impact** (low/medium/high/critical) — how bad would it be?
- **risk_type** — execution, market, technical, financial, regulatory, competitive, or people
- **mitigation** — what are we doing about it?

### Risk Lifecycle
```
Create → Assess → Mitigate → Monitor → Close/Occurred
```

Use these tools for lifecycle transitions:
- `mitigate_risk` — document mitigation actions
- `mark_risk_occurred` — the risk materialised
- `close_risk` — risk is no longer relevant
- `archive_risk` — remove from active view

### Risk Portfolio View
Use `get_high_risk_items` to see the most critical risks across the workspace.
Use `get_risk_score_distribution` to understand the overall risk profile.

## Metrics

### Setting Up Metrics
Use `create_metric` for ongoing measurements. Metrics differ from key results:
- **Metrics** are continuous (track monthly revenue forever)
- **Key Results** are time-bound (increase revenue by 20% this quarter)

### Updating Metrics
Use `update_metric` to record new values. Track the direction and rate of change, not just the absolute number.

## Execution Review Patterns

### Weekly Check
1. OKR scores updated? (`list_objectives`)
2. Any initiatives stuck? (`list_initiatives` — look for unchanged status)
3. New risks or escalated risks? (`list_risks`)
4. Assumptions needing validation? (`list_assumptions` — filter for hypothesis confidence)

### Monthly Review
1. Full OKR scoring across all objectives
2. Initiative portfolio review — reprioritise if needed
3. Assumption register audit — what's been validated/invalidated?
4. Risk register update — new risks, changed assessments
5. Metric trend analysis

### Quarterly Planning
1. Close out previous quarter's OKRs
2. Review and adjust strategy tree
3. Set new quarterly objectives
4. Define new initiatives (rocks)
5. Refresh risk and assumption registers

## When to Use This Knowledge

Activate this skill when:
- A user asks about OKRs, key results, or scoring
- Someone is reviewing initiative progress
- The conversation involves risks or assumptions
- A user wants to check strategic execution health
- Quarterly planning or review sessions
