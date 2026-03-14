# /stratafy-cpo:feature-brief

Create a feature brief grounded in the company's strategy — not just user requests.

## Input

Describe the feature:
- What it does
- Who it's for
- Why it's being considered

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "feature-brief"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` — Company context
- `list_strategies` — Which strategy does this feature serve?
- `list_initiatives` — Which initiative does it belong to?
- `list_objectives` — Which objectives will it move?
- `search_workspace` with the feature description — related context

### Step 3: Build the Brief

```
FEATURE BRIEF
━━━━━━━━━━━━━

STRATEGIC ALIGNMENT
  Strategy: [Which strategy this serves]
  Initiative: [Which initiative it belongs to]
  Objective: [Which objective it moves]

PROBLEM
  [What problem this solves — grounded in strategic context]

PROPOSED SOLUTION
  [Brief description of the feature]

SUCCESS CRITERIA
  [How to measure success — linked to objectives/metrics]

ASSUMPTIONS
  [What must be true — linked to workspace assumptions where relevant]

RISKS
  [What could go wrong — linked to workspace risks where relevant]

RECOMMENDATION
  [Build / defer / investigate further — with strategic rationale]
```
