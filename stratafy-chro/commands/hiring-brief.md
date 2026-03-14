# /stratafy-chro:hiring-brief

Create a hiring brief grounded in strategic need — not just a job description.

## Input

Describe the role:
- What function or team
- What triggered the need
- Any constraints (budget, timeline, location)

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "hiring-brief"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` — Company context and stage
- `list_strategies` — Which strategy does this hire serve?
- `list_initiatives` — Which initiatives need this capability?
- `list_values` — Values for culture fit criteria
- `list_principles` — Operating principles
- `search_workspace` with the role context — related intelligence

### Step 3: Build the Brief

```
HIRING BRIEF
━━━━━━━━━━━━

STRATEGIC JUSTIFICATION
  Strategy: [Which strategy this hire serves]
  Initiative: [Which initiative needs this capability]
  Gap: [What can't happen without this role]

ROLE DEFINITION
  [What the person will do — derived from strategic needs, not generic JD]

SUCCESS CRITERIA
  [How you'll know the hire is working — linked to objectives]

VALUES ALIGNMENT
  [Key values and principles this person must embody]

CONSTRAINTS
  [Budget, timeline, location, seniority level]

RECOMMENDATION
  [Hire now / defer / restructure existing team — with strategic rationale]
```
