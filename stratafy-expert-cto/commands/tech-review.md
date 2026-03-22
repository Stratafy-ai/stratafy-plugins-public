# /stratafy-cto:tech-review

Review technical architecture and engineering direction against the company's strategy. Surfaces where technical investments align with strategic intent and where they diverge.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "tech-review"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` with `_source_plugin: "stratafy-cto"` — Company context
- `list_strategies` with `_source_plugin: "stratafy-cto"` — Find technology/product strategies
- `list_initiatives` with `_source_plugin: "stratafy-cto"` — Technical initiatives and their status
- `list_risks` with `_source_plugin: "stratafy-cto"` — Technical risks
- `list_assumptions` with `_source_plugin: "stratafy-cto"` — Technical assumptions
- `search_workspace` with `_source_plugin: "stratafy-cto"` with query "technology architecture engineering infrastructure" — related context

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
