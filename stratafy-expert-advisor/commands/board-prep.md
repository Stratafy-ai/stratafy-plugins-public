---
description: Prepare for board meetings or investor updates with strategy-grounded narrative
---

# /stratafy-advisor:board-prep

Help prepare for a board meeting or investor update. Synthesise company data into a board-ready narrative, anticipate tough questions, and structure a memo outline.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "board-prep"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context and stage
- `list_strategies` — Strategic direction
- `list_objectives` — OKRs and goals
- `list_metrics` — Performance data
- `list_key_priorities` — Current focus areas
- `list_risks` — Risk landscape

### Step 3: Analyse for Board Readiness

Review the data through a board member's lens:

1. **Narrative coherence** — Does the strategy tell a clear story? Can you explain in 2 sentences what the company is doing and why?
2. **Metrics health** — Identify metrics that:
   - Are declining or off-target (board will ask about these)
   - Are missing entirely (board will notice gaps)
   - Tell a positive story (lead with these)
3. **Risk posture** — Are risks being actively managed or just listed? Are there existential risks the board needs to know about?
4. **Strategic progress** — Are strategies advancing or stalling? Are objectives being met?
5. **Elephants in the room** — What is the board going to ask about that the team would rather not discuss? Address it proactively

### Step 4: Prepare for Tough Questions

Anticipate 5-7 questions the board will ask, especially:
- Questions about declining metrics or missed targets
- Questions about strategic pivots or changes
- Questions about burn rate, runway, and resource allocation
- Questions about competitive threats or market shifts
- Questions about team and execution capability
- For each question, draft a concise, honest answer

### Step 5: Present Board Prep Output

```
BOARD PREP — [Workspace Name]
Meeting context: [Assessed stage, likely board composition]

LEAD WITH
  [The strongest 1-2 things to open with — positive momentum, key wins]

NARRATIVE
  [2-3 sentence strategic narrative that frames everything]

METRICS BRIEFING
  Strong:
    [Metrics that are on track or exceeding — present these confidently]
  Needs context:
    [Metrics that are off-target — with prepared explanations]
  Gaps:
    [Metrics that should exist but don't — recommend adding before the meeting]

ANTICIPATED QUESTIONS & PREPARED ANSWERS
  Q: [Expected board question]
  A: [Draft answer — honest, concise, forward-looking]

  Q: [Expected board question]
  A: [Draft answer]

  [... 5-7 questions total]

ADDRESS PROACTIVELY
  [Issues to raise before the board asks — shows self-awareness and control]

BOARD MEMO OUTLINE
  1. Executive summary (2-3 sentences)
  2. Strategic progress
     - [Strategy 1]: [status and key developments]
     - [Strategy 2]: [status and key developments]
  3. Key metrics and performance
  4. Risks and mitigations
  5. Key decisions / asks for the board
  6. Outlook and next quarter priorities
```

### Step 6: Offer Next Steps

- "Want me to draft the executive summary for the memo?"
- "Should I create an insight capturing the board prep analysis?"
- "Want me to identify which metrics need data before the meeting?"

### Provenance Context
For every mutation (create_insight, create_document, create_metric, etc.), include:
- `_source_plugin`: "stratafy-advisor"
- `_source_command`: "board-prep"
- `_change_reasoning`: Brief explanation (e.g. "Board prep identified missing metrics — creating tracking for runway and NRR before board meeting")
