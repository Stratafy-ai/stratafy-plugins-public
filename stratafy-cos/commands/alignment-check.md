# /stratafy-cos:alignment-check

Check cross-functional alignment — are all teams pulling in the same direction? Surfaces conflicts, gaps, contradictions, and misalignment between strategies, initiatives, and execution.

## When to Use

- Before quarterly planning
- When teams seem to be working at cross-purposes
- After a strategy change to check ripple effects
- When a new initiative is proposed — does it fit?
- Monthly or quarterly alignment health check

## Input

Provide one of:
- **Nothing** — Full alignment scan across all strategies
- **Two areas to compare** — "check alignment between Product and GTM"
- **A specific question** — "is our hiring plan aligned with our product roadmap?"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "alignment-check"`.

### Step 2: Load the Full Strategic Picture

In parallel:
- `get_strategy_tree` — Full hierarchy from foundation to initiatives
- `list_strategies` — All strategies with status
- `list_key_priorities` — Current priorities
- `list_initiatives` — Active initiatives
- `list_metrics` — All metrics with owners and targets
- `list_assumptions` — Assumptions underpinning strategies
- `list_risks` — Active risks

### Step 3: Run Alignment Scans

- `run_alignment_suite` — Comprehensive alignment analysis
  - Or individually: `run_alignment_foundation_scan` + `run_alignment_coherence_scan`
- Review results for specific misalignment findings

### Step 4: Cross-Functional Analysis

Check for:

**Strategy Conflicts**
- Do any strategies contradict each other? (e.g., "reduce costs" vs. "expand team")
- Are there implicit dependencies between strategies that aren't acknowledged?
- Are strategies competing for the same resources without explicit prioritisation?

**Initiative Overlap**
- Are multiple initiatives trying to achieve the same outcome?
- Are teams duplicating work without knowing it?
- Are there initiative dependencies that aren't managed?

**Metric Conflicts**
- Do any metrics pull in opposite directions? (e.g., "increase speed" and "increase quality" without acknowledging the trade-off)
- Are teams optimising for local metrics at the expense of global ones?

**Priority Mismatches**
- Do stated priorities match where resources are actually allocated?
- Are "top priority" strategies backed by adequately resourced initiatives?
- Are any key priorities orphaned — no strategy or initiative supporting them?

**Assumption Alignment**
- Are different strategies built on contradictory assumptions?
- Have any shared assumptions been invalidated?

### Step 5: Generate the Alignment Report

```
🔗 ALIGNMENT CHECK — [Date]

━━━ OVERALL HEALTH ━━━━━━━━━━━━━━━━━━━━
Alignment Score: [X/10] — [one-line summary]

━━━ MISALIGNMENTS FOUND ━━━━━━━━━━━━━━━

🔴 CONFLICT: [Area 1] vs. [Area 2]
   What: [Description of the conflict]
   Impact: [What happens if unresolved]
   Resolution options:
   1. [Option A] — [trade-off]
   2. [Option B] — [trade-off]
   Recommendation: [Your pick and why]

🟡 GAP: [What's missing]
   What: [Description of the gap]
   Why it matters: [Strategic consequence]
   Suggested fix: [Specific action]

🟡 TENSION: [Area 1] ↔ [Area 2]
   What: [Not a conflict, but a tension that needs managing]
   How to manage: [Suggested approach]

━━━ WELL ALIGNED ━━━━━━━━━━━━━━━━━━━━━━

✅ [Strategy A] ↔ [Strategy B] — [why they reinforce each other]
✅ [Priority] → [Initiative] → [Metric] — [clean alignment chain]

━━━ CROSS-FUNCTIONAL DEPENDENCIES ━━━━━

[Team A] needs [thing] from [Team B] by [date]
  Status: [on track / at risk / blocked]
  Owner: [who's responsible for the handoff]

━━━ RECOMMENDATIONS ━━━━━━━━━━━━━━━━━━━

1. [Highest priority action] — [why and who]
2. [Second priority action] — [why and who]
3. [Third priority action] — [why and who]
```

## Rules

- **Be specific about conflicts.** "Strategies are misaligned" helps no one. Name the strategies, describe the conflict, show the consequence.
- **Distinguish conflicts from tensions.** A conflict needs resolution (one side must give). A tension needs management (both sides are valid, but the balance matters).
- **Don't just find problems — recommend resolutions.** A Chief of Staff doesn't just surface issues. They propose solutions.
- **Check the invisible.** The most dangerous misalignments are the ones no one is talking about — implicit assumptions, unstated dependencies, competing metrics.
- **Acknowledge when alignment is strong.** It's not all about problems. Highlight where things are working well — it tells leadership where their energy is not needed.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
