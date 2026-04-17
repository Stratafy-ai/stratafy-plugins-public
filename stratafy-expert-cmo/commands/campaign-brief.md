---
description: Create a strategy-grounded campaign brief
---

# /stratafy-cmo:campaign-brief

Create a campaign brief that's grounded in the company's actual strategy — not generic marketing best practice.

## Input

Provide one of:
- A campaign idea — "launch campaign for the new feature"
- A strategic goal — "we need more enterprise leads"
- A market moment — "competitor just raised, we need to respond"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "campaign-brief"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` — Company context
- `list_strategies` — Which strategy does this campaign serve?
- `list_initiatives` — Active initiatives that might drive the campaign
- `list_key_priorities` — Current company priorities
- `search_workspace` with the campaign topic — find related strategic context

### Step 3: Build the Brief

```
CAMPAIGN BRIEF
━━━━━━━━━━━━━━

STRATEGIC ALIGNMENT
  Strategy: [Which strategy this serves]
  Initiative: [Which initiative drives it]
  Priority fit: [How it connects to current priorities]

OBJECTIVE
  [What this campaign should achieve — grounded in strategy, not vanity]

TARGET AUDIENCE
  [Derived from strategy and market context]

KEY MESSAGE
  [Core message that serves both the campaign goal and the strategy]

SUCCESS METRICS
  [Metrics that matter — linked to strategic objectives where possible]

CONSTRAINTS
  [What's out of scope, budget considerations, timing]
```

### Step 4: Persist the Brief

Render the brief body to the user first. Then persist it so it survives the chat and becomes a first-class workspace entity:

1. **Create a draft.** Call `create_brief` with:
   - `brief_type: "campaign"`
   - `title`: "Campaign Brief — [campaign name/topic]"
   - `description`: the campaign objective in one line
   - `content`: the rendered markdown body from Step 3
   - `source_plugin: "stratafy-cmo"`
   - `source_command: "campaign-brief"`
   - `lead_strategy_id` and `lead_initiative_id` from Step 2
   - Plus provenance.

2. **Release it.** Call `release_brief` with the brief `id` plus:
   - `content`: final body
   - `generation_qa`: `[{ question, answer }]` pairs if relevant. Empty array otherwise.
   - `context_refs`: `{ strategy_ids, initiative_ids, metric_ids }` — IDs from Step 2.
   - `context_snapshot`: strategic alignment summary (strategy + priority fit).
   - Same provenance.

3. **Show the persistent URL.** Tell the user: "Saved as brief `<brief_id>` — `https://stratafy.ai/ws/<workspace_id>/brief/<brief_id>`. This is now a first-class workspace entity: versioned, shareable, trackable."

### Step 5: Offer Next Steps

- "Want me to log this as a decision?"
- "Should I create an initiative for this campaign?"

### Provenance Context
For every mutation (create_decision, create_initiative, etc.), include:
- `_source_plugin`: "stratafy-cmo"
- `_source_command`: "campaign-brief"
- `_change_reasoning`: Brief explanation (e.g. "Campaign brief led to new initiative — Q2 product launch campaign aligned to growth strategy")
