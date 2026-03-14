# /stratafy-chro:team-health

Review team capacity and health against strategic demands. Surfaces where the team is stretched, where there are gaps, and what the strategy requires.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "team-health"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context
- `list_strategies` — Active strategies and their leads
- `list_initiatives` — Active initiatives and ownership
- `list_key_priorities` — Current priorities
- `list_values` — Company values (culture alignment)
- `search_workspace` with query "team capacity hiring culture retention" — related context

### Step 3: Assess

- Are all active strategies adequately resourced with leads?
- Are there initiatives without clear ownership?
- Is work concentrated on too few people?
- Do the priorities require capabilities the team doesn't have?

### Step 4: Present

```
TEAM HEALTH — [Workspace Name]

CAPACITY ALIGNMENT
  [Which strategies are resourced vs under-resourced]

OWNERSHIP GAPS
  [Strategies or initiatives without clear leads]

CONCENTRATION RISK
  [Where too much depends on too few people]

CAPABILITY GAPS
  [Strategic priorities that require skills the team may lack]

RECOMMENDATIONS
  1. [Specific action]
  2. [Specific action]
```
