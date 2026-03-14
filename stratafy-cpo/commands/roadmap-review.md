# /stratafy-cpo:roadmap-review

Review the product roadmap against the company's strategy. Surfaces where product work aligns with strategic intent and where it doesn't.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "roadmap-review"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context
- `list_strategies` — Find product strategies
- `list_initiatives` — Active product initiatives
- `list_objectives` — Product objectives and their progress
- `list_key_priorities` — Current priorities
- `search_workspace` with query "product roadmap features priorities" — related context

### Step 3: Assess Alignment

For each product initiative:
- Does it serve an active strategy?
- Is it connected to measurable objectives?
- Does its priority match the strategic priority?
- Are there strategies with no product initiatives driving them?

### Step 4: Present

```
ROADMAP REVIEW — [Workspace Name]

STRATEGIC ALIGNMENT
  [Which product initiatives serve which strategies]

COVERAGE GAPS
  [Strategies with no product initiatives driving them]

PRIORITY ALIGNMENT
  [Where initiative priority matches/mismatches strategic priority]

OBJECTIVE HEALTH
  [Product objectives — on track, at risk, off track]

RECOMMENDATIONS
  1. [Specific action]
  2. [Specific action]
```
