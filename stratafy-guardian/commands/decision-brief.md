---
name: decision-brief
description: Prepare a comprehensive context package for a strategic decision
---

# /stratafy:decision-brief

Prepare a comprehensive context package for a strategic decision. Gathers all relevant information — related strategies, risks, assumptions, signals, and insights — so the decision-maker has everything they need.

## When to Use

- A decision is pending and needs stakeholder input
- Preparing a recommendation for leadership or the board
- A coaching client needs to make a strategic choice
- Multiple options need to be evaluated with full context
- An advisor or board member needs to weigh in

## Input

Provide one of:
- **A pending decision** — "brief me on the pricing model decision"
- **A new decision to frame** — "we need to decide whether to enter the European market"
- **A decision ID** — if you know the Stratafy decision ID

## Process

### Step 1: Identify or Create the Decision
If the decision exists, I'll use `get_decision` to load it.
If it's new, I'll use `create_decision` to log it with:
- Clear title and description
- Decision type (type_1_irreversible or type_2_reversible)
- Status as `pending`

### Step 2: Gather Context
I'll pull all relevant information from the workspace:

**Strategic context:**
- `get_strategy_tree` — Where does this decision sit in the strategic architecture?
- Which strategies does it affect or depend on?
- Does it align with the foundation (mission, vision, values)?

**Assumptions at stake:**
- `get_assumptions_for_context` — What assumptions are relevant to this decision?
- Which assumptions does each option depend on?
- Are any of those assumptions invalidated or untested?

**Risk landscape:**
- `get_risks_for_context` — What risks relate to this decision?
- What new risks does each option introduce?
- What risks does each option mitigate?

**Signal intelligence:**
- `get_signals_for_strategy` — What signals from the environment are relevant?
- Do any signals favour one option over another?

**Related insights:**
- `search_insights` — What do we already know that's relevant?
- Are there past decisions that set precedent?

**Related decisions:**
- `list_decisions` — Any related decisions already made or pending?
- Are there dependencies between this decision and others?

### Step 3: Frame the Options
For each option, I'll present:
- **Description** — What does this option entail?
- **Strategic alignment** — How well does it align with current strategy and foundation?
- **Assumptions required** — What must be true for this option to succeed?
- **Risks introduced** — What new risks does this option create?
- **Risks mitigated** — What existing risks does this option address?
- **Signal support** — Do environmental signals favour this option?
- **Reversibility** — How easily can we reverse this decision if it's wrong?
- **Resource implications** — What does this option cost in time, money, and attention?

### Step 4: Generate Recommendation
Based on the full context, I'll provide:
- A clear recommendation with rationale
- Confidence level in the recommendation
- Key factors that could change the recommendation
- Suggested decision timeline

### Step 5: Decision Brief Document

```
DECISION BRIEF
Title: [Decision title]
Type: [Type 1 (irreversible) / Type 2 (reversible)]
Date: [today]
Status: Pending

CONTEXT
[2-3 paragraphs on why this decision is needed now]

OPTIONS
Option A: [Name]
  Alignment: [High/Medium/Low]
  Key assumptions: [list]
  Key risks: [list]
  Reversibility: [High/Medium/Low]

Option B: [Name]
  [same structure]

RECOMMENDATION
[Clear recommendation with rationale]

CONFIDENCE: [High/Medium/Low]
KEY DEPENDENCIES: [What must be true]
SUGGESTED TIMELINE: [When to decide by]

DECISION RECORD
[Space for the actual decision and rationale once made]
```

## After the Decision

Once a decision is made, I'll:
- Update the decision in Stratafy with `update_decision`
- Set status to `decided` with the rationale
- Create any resulting risks or assumptions
- Update affected strategies or initiatives
- Log any new signals or insights that emerged from the discussion

### Provenance Context
For every mutation (create_decision, update_decision, create_risk, etc.), include:
- `_source_plugin`: "stratafy-guardian"
- `_source_command`: "decision-brief"
- `_change_reasoning`: Brief explanation tied to the decision context (e.g. "Decision framed during brief — logging pricing model choice with rationale")

## For Coaches
- Use decision briefs to structure client thinking around important choices
- The framework (options, assumptions, risks) teaches strategic rigour
- Present the brief before a session so the client comes prepared
- After the session, update the decision record with the client's choice and rationale

## For Advisors & Board Members
- Decision briefs are designed to be self-contained — an advisor can read one without deep workspace knowledge
- The strategic context section provides enough background to contribute meaningfully
- The options framework ensures all perspectives are considered
