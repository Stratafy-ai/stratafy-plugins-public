# /stratafy-cmo:brand-audit

Audit brand positioning and messaging against the current strategy. Surfaces misalignment between what the company says and what the strategy intends.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "brand-audit"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context and industry
- `list_strategies` — Active strategies and positioning
- `list_values` — Company values
- `list_principles` — Operating principles
- `search_workspace` with query "brand positioning messaging target audience" — find any documented brand thinking

### Step 3: Assess Alignment

For each active strategy, evaluate:
- Does the company's stated positioning support this strategy?
- Are there messaging gaps — strategies with no corresponding market narrative?
- Do the values and principles show up in the brand voice?

### Step 4: Present Findings

```
BRAND AUDIT — [Workspace Name]

POSITIONING ALIGNMENT
  [Strategy] → [How current positioning serves/doesn't serve it]

MESSAGING GAPS
  [Strategies or initiatives with no clear market narrative]

VOICE CONSISTENCY
  [Do values and principles show up in how the company presents itself?]

RECOMMENDATIONS
  1. [Specific action]
  2. [Specific action]
```

### Step 5: Offer Next Steps

- "Want me to draft positioning statements for the gaps?"
- "Should I create an insight capturing the brand alignment issues?"
