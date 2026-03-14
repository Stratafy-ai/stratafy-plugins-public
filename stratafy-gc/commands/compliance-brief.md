# /stratafy-gc:compliance-brief

Assess compliance considerations for a specific strategic initiative — before it launches, not after.

## Input

Describe the initiative:
- What it involves
- What markets or jurisdictions
- What data or assets are involved

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "compliance-brief"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` — Company context and industry
- `list_strategies` — Which strategy does this initiative serve?
- `list_risks` — Existing risks that may apply
- `list_assumptions` — Assumptions about the regulatory environment
- `search_workspace` with the initiative context — related intelligence

### Step 3: Build the Brief

```
COMPLIANCE BRIEF
━━━━━━━━━━━━━━━━

STRATEGIC CONTEXT
  Strategy: [Which strategy this initiative serves]
  Initiative: [What's being assessed]

COMPLIANCE CONSIDERATIONS
  [Key areas of potential regulatory exposure]

EXISTING RISKS
  [Workspace risks that apply to this initiative]

ASSUMPTIONS TO VALIDATE
  [Regulatory assumptions that need checking]

RISK LEVEL
  [Low / Medium / High — with rationale]

RECOMMENDED ACTIONS
  1. [Specific action — e.g., "seek legal opinion on X"]
  2. [Specific action]

This is a strategic risk assessment, not legal advice.
   Consult professional counsel for binding decisions.
```
