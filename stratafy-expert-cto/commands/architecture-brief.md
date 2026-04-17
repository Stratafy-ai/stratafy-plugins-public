---
description: Create an architecture decision brief grounded in strategic context
---

# /stratafy-cto:architecture-brief

Create an architecture decision brief grounded in strategic context — not just technical merit.

## Input

Describe the decision:
- What architectural choice is being considered
- What problem it solves
- What alternatives exist

## Process

### Step 1: Get User Context & Expert Profile

In parallel:
- Call `get_user_context` with `command_name: "architecture-brief"`, `plugin_name: "stratafy-cto"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert` with `role: "cto"` — returns expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step.

### Step 2: Gather Strategic Context

From Step 1 results, identify the **most relevant active strategy** for the decision being considered.

In parallel:
- `get_strategy` with the relevant strategy ID — full content and health
- `list_initiatives` filtered by `strategy_id` — related technical initiatives
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — risks this decision creates or mitigates
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — assumptions that bear on the decision
- `search_workspace` with the decision context — any prior thinking or decisions

**Do NOT call `get_workspace_snapshot`, `list_strategies`, `list_risks`, or `list_assumptions` without filters — these return oversized payloads.**

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

### Step 4: Persist the Brief

Render the brief body to the user first. Then persist it so it survives the chat and becomes a first-class workspace entity:

1. **Create a draft.** Call `create_brief` with:
   - `brief_type: "architecture"`
   - `title`: the decision summary as a short title
   - `description`: one-line summary of the decision
   - `content`: the rendered markdown body from Step 3
   - `source_plugin: "stratafy-expert-cto"`
   - `source_command: "architecture-brief"`
   - `lead_strategy_id`: the strategy identified in Step 2
   - Plus provenance: `_source_plugin`, `_source_command`, `_change_reasoning` (1 sentence on why this decision is being captured), `_intent: "user_request"`, `_llm_model`.

2. **Release it.** Call `release_brief` with the brief `id` returned above, plus:
   - `content`: the final body (same as create, or the user-polished version)
   - `generation_qa`: clarifying questions you asked the user during generation as `[{ question, answer }]`. Empty array if none.
   - `context_refs`: the entity IDs you pulled in Step 2 — `{ strategy_ids, initiative_ids, risk_ids, assumption_ids, decision_ids }`. Only include arrays for entity types actually referenced.
   - `context_snapshot`: 3–5 line text summary of the strategic context at release time (strategy name + status, key initiatives, any load-bearing risks/assumptions). Allows future readers to reconstruct what was true.
   - `release_note`: optional 1-line note for audit history.
   - Same standard provenance.

3. **Show the persistent URL.** Tell the user: "Saved as brief `<brief_id>` — `https://stratafy.ai/ws/<workspace_id>/brief/<brief_id>`. This is now a first-class workspace entity: versioned, shareable, trackable."

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "architecture-brief"
- `_change_reasoning`: brief explanation
