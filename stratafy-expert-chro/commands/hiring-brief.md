---
description: Create a hiring brief grounded in strategic need
---

# /stratafy-expert-chro:hiring-brief

Create a hiring brief grounded in strategic need — not just a job description. Every hire should trace to a strategy.

## When to Use

- Planning a new hire and need strategic justification
- Writing a job brief that connects to company strategy
- Evaluating whether to hire, restructure, or outsource
- Building a case for headcount to leadership

## Input

Describe the role:
- What function or team
- What triggered the need
- Any constraints (budget, timeline, location)

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "hiring-brief"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Strategic Context

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]` — company context and strategic landscape
- `list_strategies` — which strategy does this hire serve?
- `list_initiatives` — which initiatives need this capability?
- `list_values` — values for culture fit criteria
- `list_principles` — operating principles
- `search_workspace` with the role context — related intelligence

### Step 3: Build the Brief

Analyse the strategic context against the hiring need:
- Which strategy does this role serve? If none, challenge the hire.
- Which initiatives are blocked or understaffed without this capability?
- What can't happen without this role that the strategy requires?
- Is hiring the right answer, or could restructuring or outsourcing close the gap?

### Step 4: Present

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

### Step 5: Persist the Brief

Render the brief body to the user first. Then persist it so it survives the chat and becomes a first-class workspace entity:

1. **Create a draft.** Call `create_brief` with:
   - `brief_type: "hiring"`
   - `title`: "Hiring Brief — [role title]"
   - `description`: one-line summary of the role and strategic need
   - `content`: the rendered markdown body from Step 4
   - `source_plugin: "stratafy-expert-chro"`
   - `source_command: "hiring-brief"`
   - `lead_strategy_id` and `lead_initiative_id` from Step 2
   - Plus provenance: `_source_plugin`, `_source_command`, `_change_reasoning`, `_intent: "user_request"`, `_llm_model`.

2. **Release it.** Call `release_brief` with the brief `id` plus:
   - `content`: final body
   - `generation_qa`: `[{ question, answer }]` if you asked the user about constraints or trade-offs. Empty array otherwise.
   - `context_refs`: `{ strategy_ids, initiative_ids, objective_ids }` — IDs from Step 2.
   - `context_snapshot`: 3–5 line summary of strategic context (strategy + initiative + gap).
   - Same provenance.

3. **Show the persistent URL.** Tell the user: "Saved as brief `<brief_id>` — `https://stratafy.ai/ws/<workspace_id>/brief/<brief_id>`. This is now a first-class workspace entity: versioned, shareable, trackable."

### Step 6: Offer Follow-ups

- "Want me to draft the role description?" — deeper JD based on the brief
- "Want to check team capacity first?" — `/stratafy-expert-chro:team-health`
- "Want to see org alignment?" — `/stratafy-expert-chro:org-review`

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "hiring-brief"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Strategy-grounded.** Every hire must trace to a strategy. If it doesn't, challenge it.
2. **Alternatives first.** Before recommending a hire, consider restructuring, outsourcing, or reprioritising.
3. **Values matter.** Culture fit criteria come from the workspace's actual values, not generic competencies.
4. **Honest.** If the hire doesn't make strategic sense, say so — even if that's not what the user wants to hear.
5. **Concrete.** Success criteria are measurable and linked to strategic objectives, not vague ("good culture fit").
6. When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
