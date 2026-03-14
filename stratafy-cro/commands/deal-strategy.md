# /stratafy-cro:deal-strategy

Build a strategy-aligned approach for a specific deal or opportunity.

## Input

Describe the deal:
- Company/prospect name
- What they need
- Deal size and stage
- Any concerns or blockers

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "deal-strategy"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` — Company positioning and context
- `list_strategies` — Which strategy does this deal serve?
- `list_assumptions` — Assumptions about this market segment
- `search_workspace` with the deal/prospect context — related intelligence

### Step 3: Build Deal Strategy

```
DEAL STRATEGY — [Prospect Name]

STRATEGIC FIT
  Strategy: [Which strategy this deal serves]
  Fit score: [High/Medium/Low — based on strategic alignment]

POSITIONING
  [How to position based on company strategy, not generic pitch]

KEY ASSUMPTIONS
  [What must be true for this deal to succeed — linked to workspace assumptions]

RISKS
  [Deal-specific risks, linked to strategic risks where relevant]

APPROACH
  [Recommended approach grounded in strategic positioning]

NEXT STEP
  [Single concrete action]
```
