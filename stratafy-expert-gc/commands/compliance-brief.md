---
description: Assess compliance considerations for a strategic initiative
---

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

### Step 4: Persist the Brief

Render the brief body to the user first. Then persist it so it survives the chat and becomes a first-class workspace entity:

1. **Create a draft.** Call `create_brief` with:
   - `brief_type: "compliance"`
   - `title`: "Compliance Brief — [initiative name]"
   - `description`: the risk level and one-line rationale
   - `content`: the rendered markdown body from Step 3
   - `source_plugin: "stratafy-expert-gc"`
   - `source_command: "compliance-brief"`
   - `lead_strategy_id` and `lead_initiative_id` from Step 2
   - Plus provenance: `_source_plugin`, `_source_command`, `_change_reasoning`, `_intent: "user_request"`, `_llm_model`.

2. **Release it.** Call `release_brief` with the brief `id` plus:
   - `content`: final body
   - `generation_qa`: `[{ question, answer }]` pairs if you asked about jurisdictions or data types. Empty array otherwise.
   - `context_refs`: `{ strategy_ids, initiative_ids, risk_ids, assumption_ids }` — IDs from Step 2.
   - `context_snapshot`: the compliance considerations + risk level section.
   - Same provenance.

3. **Show the persistent URL.** Tell the user: "Saved as brief `<brief_id>` — `https://stratafy.ai/ws/<workspace_id>/brief/<brief_id>`. This is now a first-class workspace entity: versioned, shareable, trackable."
