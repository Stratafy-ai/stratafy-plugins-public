# /stratafy-cto:architecture-brief

Create an architecture decision brief grounded in strategic context — not just technical merit.

## Input

Describe the decision:
- What architectural choice is being considered
- What problem it solves
- What alternatives exist

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "architecture-brief"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` — Company context and stage
- `list_strategies` — Which strategy does this decision serve?
- `list_initiatives` — Related technical initiatives
- `list_assumptions` — Assumptions that bear on the decision
- `list_risks` — Risks this decision creates or mitigates
- `search_workspace` with the decision context — related intelligence

### Step 3: Build the Brief

```
ARCHITECTURE DECISION BRIEF
━━━━━━━━━━━━━━━━━━━━━━━━━━━

STRATEGIC CONTEXT
  Strategy: [Which strategy this serves]
  Stage: [Company stage — affects build-vs-buy calculus]

DECISION
  [What's being decided]

OPTIONS
  Option A: [Description]
    Strategic fit: [How it serves/constrains strategy]
    Risk profile: [What risks it creates or mitigates]

  Option B: [Description]
    Strategic fit: [...]
    Risk profile: [...]

ASSUMPTIONS
  [What must be true — linked to workspace assumptions]

RECOMMENDATION
  [Which option and why — grounded in strategic rationale, not just technical merit]

NEXT STEP
  [Single concrete action]
```
