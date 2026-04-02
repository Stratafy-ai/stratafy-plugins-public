---
description: Review technical architecture against company strategy
---

# /stratafy-cto:tech-review

Review technical architecture and engineering direction against the company's strategy. Surfaces where technical investments align with strategic intent and where they diverge.

## Process

### Step 1: Get User Context & Expert Profile

In parallel:
- Call `get_user_context` with `command_name: "tech-review"`, `plugin_name: "stratafy-cto"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert` with `role: "cto"` — returns expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step.

### Step 2: Gather Context

From Step 1 results, filter to **active strategies only**.

For each active owned strategy, in parallel:
- `get_strategy` — full strategy content, health score, alerts
- `list_initiatives` filtered by `strategy_id` — technical initiatives and their status
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — technical risks
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — technical assumptions

Also in parallel (not per-strategy):
- `list_key_priorities` — current company priorities to check tech alignment
- `search_workspace` with query "technology architecture engineering infrastructure" — related context

**Do NOT call `get_workspace_snapshot`, `list_risks`, `list_assumptions`, or `list_initiatives` without filters — these return oversized payloads.**

### Step 3: Assess

- Are technical initiatives serving active strategies?
- Are there strategies that need technical enablement but have none?
- What technical risks threaten strategic execution?
- Are technical assumptions still valid?

### Step 4: Present

```
TECH REVIEW — [Workspace Name]

STRATEGIC ALIGNMENT
  [Which technical initiatives serve which strategies]

ENABLEMENT GAPS
  [Strategies that need technical work but don't have it]

TECHNICAL RISKS
  [Risks that threaten strategic execution]

ASSUMPTION HEALTH
  [Technical assumptions — still valid, needs validation, invalidated]

RECOMMENDATIONS
  1. [Specific action]
  2. [Specific action]
```

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "tech-review"
- `_change_reasoning`: brief explanation
