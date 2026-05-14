---
description: Scan the org's distributed-work operating model for strategic drift — heartbeat, dormancy, decision velocity, ownership distribution, value staging
---

# /stratafy-expert-chro:hybrid-health

Diagnostic scan of how the org's distributed/hybrid working model is supporting (or quietly undermining) strategic execution. Not an engagement survey — a structural read on whether async cadences, decision flow, ownership distribution, and value-staging are keeping a distributed team aligned without sync-meeting tax.

The frame is trust-based, outcomes-over-presence: the platform observes structural heartbeats, surfaces silence, and flags drift. It does not enforce, surveil, or score individuals.

## When to Use

- Quarterly people-health review on a distributed-by-default org
- Before / after a hub expansion or contraction
- When decision velocity feels slow or strategies feel dormant
- When the gap between stated and lived values needs evidence
- Before an exec-team conversation about "are we still working the way we say we work?"

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "hybrid-health"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Context

In parallel:

- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities", "decisions", "initiatives"]` — full strategic landscape with the entities this scan needs to read heartbeat against
- `list_strategies` — active strategies and their leads (ownership + last-touched signals)
- `list_initiatives` — initiative ownership and completion data
- `list_decisions` — pending decisions and their age (async decision-flow signal)
- `list_check_ins` — recent check-ins per strategy / initiative (heartbeat signal)
- `list_values` — values with `cost_something_to_uphold` flags + examples (value-staging signal)

### Step 3: Assess (five lenses)

Analyse the data through five distributed-work lenses:

**Heartbeat & dormancy.** For each active strategy and initiative, compute a heartbeat score from: last `check_in` referencing it, `updated_at`, recent Decisions referencing it, recent insights / signals linked to it. Anything silent across all signals for 30+ days is dormant. 60+ days is critical.

**Async decision flow.** For all Decisions in `pending` state, compute median age (in days) and surface any older than 14 days. Long pending-age usually signals sync-meeting bottleneck — the highest-leverage thing distributed orgs fix.

**Ownership distribution.** For each active strategy, identify the owner. Surface single-owner strategies (concentration risk) and — if hub / location data is available on the user profile — surface geographic concentration (e.g., "70% of strategies owned in one timezone").

**Value staging.** For each Value, check whether `cost_something_to_uphold` is set AND whether the value is referenced by any Decision in the last 90 days. A value declared but never staged in a Decision is cosmetic by definition.

**Cross-hub collaboration.** If team-member hub data is queryable, identify initiatives whose team spans only one hub vs ≥2. A high single-hub ratio in a stated-distributed org is a signal that hybrid is happening on paper but not in practice.

If hub / location data is unavailable from the workspace, surface the dimension as "not measurable from current data" rather than skipping silently. Honest gaps beat false signals.

### Step 4: Present

```
HYBRID HEALTH SCAN — [Workspace Name]
[Date]  ·  trust-based, observation-only

━━━ HEARTBEAT ━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ [N] of [M] active strategies have heartbeat in last 14 days
⚠ [N] strategies dormant — no signal across all sources in 30+ days
   → [Strategy Title] — [N] days silent
   → [Strategy Title] — [N] days silent
✗ [N] strategies critical-dormant — 60+ days silent, likely need
   a Decision to keep or archive
   → [Strategy Title] — [N] days silent

━━━ ASYNC DECISION FLOW ━━━━━━━━━━━━━━━━
- Median pending Decision age: [N] days [healthy / watch / drift]
- Oldest pending: [N] days — [Decision Title]
- [N] pending Decisions older than 14 days
- [Pattern observation if any — e.g. "all 4 oldest are in one functional area"]

━━━ OWNERSHIP DISTRIBUTION ━━━━━━━━━━━━━
- [N] of [M] active strategies have a single owner — concentration risk
   → [Strategy Title] — sole owner: [Owner Name]
   → [Strategy Title] — sole owner: [Owner Name]
- Geographic distribution: [if measurable, otherwise "not measurable from current data"]

━━━ VALUE STAGING ━━━━━━━━━━━━━━━━━━━━━━
For each value:
   [VALUE NAME] — cost_something_to_uphold: [yes/no/not declared]
                — referenced in [N] Decisions in last 90 days
                — [status: staged / declared-but-not-staged / undeclared]

━━━ CROSS-HUB COLLABORATION ━━━━━━━━━━━━
[If measurable:]
- [N] of [M] active initiatives have team from ≥2 hubs
- Single-hub initiatives: [N] — [list top 3]
[Else:]
- Not measurable from current workspace data. To enable, add hub field
  to user profiles and team-member assignments.

━━━ TOP 3 ACTIONS ━━━━━━━━━━━━━━━━━━━━━━
1. [Specific action with strategic rationale]
2. [Specific action with strategic rationale]
3. [Specific action with strategic rationale]
```

### Step 5: Offer Follow-ups

- "Want to drill into a specific dormant strategy?" — point to that strategy's detail URL
- "Want to check team capacity against this picture?" — `/stratafy-expert-chro:team-health`
- "Want to test culture alignment against the staged values?" — `/stratafy-expert-chro:culture-check`
- "Want to scan retention risk on the concentration-risk owners?" — `/stratafy-expert-chro:retention-scan`

## Provenance Context

On every mutation (Insight / Risk / Decision created from this scan), include:

- `_source_plugin`: `"stratafy-expert-chro"`
- `_source_command`: `"hybrid-health"`
- `_change_reasoning`: brief explanation (e.g. "Capturing dormancy on Strategy X surfaced during hybrid-health scan")

## Rules

1. **Observe, do not enforce.** This is the load-bearing principle. The platform reads heartbeats; humans decide whether to add structure. Anything that reads as surveillance or scoring is a trust-pitch failure.
2. **No per-employee scoring.** This scan reads workspace entities, not people. If the data shape would force a per-employee signal (e.g. login frequency, hours active), exclude it entirely. Aggregate or skip.
3. **Honest gaps over false signals.** If a dimension can't be measured from current workspace data (commonly: hub assignment, async-meeting tax), say so explicitly. Don't pretend.
4. **Strategy-linked.** Every finding ties to a strategy, initiative, or Value. "Decision flow is slow" becomes "median pending Decision age 18 days across these 4 strategies."
5. **Dormancy is signal, not verdict.** A dormant strategy may be correctly resting between cycles. Surface it, propose a Decision to keep / archive / resume, let the human decide.
6. **Value staging is the cosmetic-culture detector.** Values declared but never staged in any Decision in 90 days are the failure mode the methodology's `cost_something_to_uphold` field exists to expose. Be precise — name the values, name the Decision count.
7. **Use markdown links with `urls.detail`** when referencing strategies, initiatives, Decisions — so the user can drill in.

## Why this command exists

Distributed and hybrid work models scale when async cadences + ownership clarity + ritual hooks replace office-by-osmosis. They quietly collapse when those substrates are missing. This scan reads exactly those substrates from workspace structure — heartbeat, decision flow, ownership distribution, value staging, cross-hub collaboration — and surfaces drift before it shows up in attrition or engagement metrics.

It complements `/stratafy-expert-chro:team-health` (capacity vs strategic demand) and `/stratafy-expert-chro:culture-check` (values vs decisions). Where those answer "is the team big enough?" and "are we walking our talk?", this answers "is our distributed operating model actually working?"
