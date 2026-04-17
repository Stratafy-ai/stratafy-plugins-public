---
description: Create a strategy-grounded feature brief
---

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

### Step 4: Persist the Brief

Render the brief body to the user first. Then persist it so it survives the chat and becomes a first-class workspace entity:

1. **Create a draft.** Call `create_brief` with:
   - `brief_type: "feature"`
   - `title`: short title summarising the feature
   - `description`: one-line summary
   - `content`: the rendered markdown body from Step 3
   - `source_plugin: "stratafy-expert-cpo"`
   - `source_command: "feature-brief"`
   - `lead_strategy_id` and `lead_initiative_id`: the strategy/initiative identified in Step 2
   - Plus provenance: `_source_plugin`, `_source_command`, `_change_reasoning`, `_intent: "user_request"`, `_llm_model`.

2. **Release it.** Call `release_brief` with the brief `id` plus:
   - `content`: final body
   - `generation_qa`: `[{ question, answer }]` pairs if you interviewed the user. Empty array if none.
   - `context_refs`: IDs from Step 2 — `{ strategy_ids, initiative_ids, objective_ids, risk_ids, assumption_ids }`. Only include arrays actually referenced.
   - `context_snapshot`: 3–5 line text summary of strategic context at release time.
   - Same provenance.

3. **Show the persistent URL.** Tell the user: "Saved as brief `<brief_id>` — `https://stratafy.ai/ws/<workspace_id>/brief/<brief_id>`. This is now a first-class workspace entity: versioned, shareable, trackable."

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-cpo"
- `_source_command`: "feature-brief"
- `_change_reasoning`: brief explanation of why the change is being made
