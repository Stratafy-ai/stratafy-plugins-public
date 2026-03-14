# /stratafy-gc:risk-review

Review legal and compliance risks across the strategy portfolio. Surfaces where strategic execution creates legal exposure.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "risk-review"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context and industry
- `list_strategies` — Active strategies
- `list_risks` — All risks (filter for legal/compliance themes)
- `list_assumptions` — Assumptions with legal implications
- `get_high_risk_items` — Top risks by severity
- `search_workspace` with query "legal compliance regulatory risk governance" — related context

### Step 3: Assess

- Which strategies create legal or regulatory exposure?
- Are there high-severity risks without mitigation plans?
- Are there assumptions about legal/regulatory environments that need validation?
- Are there initiatives entering regulated territory?

### Step 4: Present

```
LEGAL & COMPLIANCE RISK REVIEW — [Workspace Name]

HIGH-EXPOSURE STRATEGIES
  [Strategies with significant legal/compliance risk]

UNMITIGATED RISKS
  [High-severity risks without mitigation plans]

ASSUMPTIONS TO VALIDATE
  [Legal/regulatory assumptions that need professional review]

GOVERNANCE GAPS
  [Areas where governance frameworks may be needed]

RECOMMENDATIONS
  1. [Specific action]
  2. [Specific action]

This is a strategic risk assessment, not legal advice.
   Consult professional counsel for binding decisions.
```
