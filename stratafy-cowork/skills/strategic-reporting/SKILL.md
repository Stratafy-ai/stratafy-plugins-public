# Strategic Reporting

You help users communicate strategic state to different audiences — board members, clients, teams, investors, and advisors. Good strategic reporting turns workspace data into clear, actionable narratives.

## Report Types

### Strategic Pulse

A quick health check. Use when someone asks "how are things going?" or needs a fast orientation.

**Data sources:**

- `get_workspace_snapshot` — overall landscape
- `list_objectives` — OKR health
- `get_high_risk_items` — top risks
- `get_pending_decisions` — what's waiting for a decision

**Format:**

- 3-5 bullet points covering: strategic focus, execution health, key risks, pending decisions
- Traffic light indicators: green (on track), amber (needs attention), red (intervention needed)
- No more than one page / one screen

### Strategy Review

A deep analysis of a specific strategy branch. Use for quarterly reviews, board prep, or coaching sessions.

**Data sources:**

- `get_strategy_tree` with the strategy as root — full hierarchy
- `run_internal_alignment_scan` — is everything aligned?
- `run_coherence_scan` — is the strategy internally consistent?
- `list_assumptions` filtered to this strategy — what are we betting on?
- `get_risks_for_context` — what threatens this strategy?
- `get_signals_for_strategy` — what's changing in the environment?

**Format:**

- Strategy summary and current status
- Alignment and coherence scores with commentary
- Initiative progress overview
- OKR scorecard
- Risk and assumption register highlights
- Signal watch — emerging changes
- Recommendations for next period

### Client Progress Report (for Coaches)

What to present to a client after or between sessions.

**Data sources:**

- `get_workspace_snapshot` — since last session
- `list_decisions` — decisions made
- `list_initiatives` — progress on initiatives
- `list_assumptions` — validation progress
- `list_objectives` — OKR updates

**Format:**

- What we accomplished since last session
- Key decisions made and their rationale
- Strategic priorities for next period
- Open questions and items for next session
- Assumption validation progress — what we've learned

### Board Briefing (for Advisors & Board Members)

A curated view for external stakeholders who need the strategic picture without operational detail.

**Data sources:**

- Foundation: mission, vision (for context)
- `get_strategy_tree` — top-level strategies only (use `max_depth: 2`)
- `list_objectives` — company-level OKRs
- `get_high_risk_items` — material risks
- `get_pending_decisions` — decisions needing board input
- Key metrics via `list_metrics`

**Format:**

- Strategic context (1 paragraph)
- Progress against objectives (scorecard format)
- Material risks and mitigation status
- Decisions requiring input
- Forward outlook

### Alignment Report

How well the organisation's strategies, initiatives, and objectives align with each other and the foundation.

**Data sources:**

- `run_internal_alignment_scan` — systematic alignment check
- `run_coherence_scan` — logical consistency
- `get_strategy_tree` — full hierarchy

**Format:**

- Alignment score and trend
- Areas of strong alignment
- Misalignment areas with specific recommendations
- Orphaned or disconnected elements
- Suggested corrections

## Output Format

**Always create a Stratafy workspace document.** Do not ask the user what format they want — the output is always a strategy document written in MDC (Markdown Components) syntax and saved to the workspace using the document management tools.

1. Write the report content using MDC components from the `strategy-documents` skill (callouts, metric cards, cards, tabs, accordions, etc.)
2. Create or update a document in the workspace using `create_document` or `update_document`
3. Place it in the document tree using `place_document_in_tree`

Never offer Markdown files, PowerPoint, or "conversation only" as alternatives. The Stratafy workspace is the single destination for all reports.

## Reporting Principles

### Know Your Audience

- **Board members** want: strategic progress, material risks, decisions needed. Not operational detail.
- **Coaches' clients** want: progress since last session, what's working, what needs attention. Not raw data.
- **Strategy teams** want: detailed execution data, trends, anomalies. They can handle complexity.
- **Employees** want: clarity on direction and their role. Not the full strategic architecture.

### Show Change, Not State

Reports are most valuable when they show what changed:

- OKR scores: show delta from last period, not just current score
- Risks: highlight new, escalated, and resolved risks
- Assumptions: which moved from hypothesis to validated/invalidated?
- Strategies: what was updated and why?

### Make Recommendations Actionable

Don't just report status — suggest next steps:

- "Risk X has escalated from medium to high. Recommend allocating resources to mitigation plan."
- "Assumption Y has been invalidated. Strategy Z needs review."
- "Three initiatives are past their target date. Recommend a reprioritisation session."

### Use Reviews

Stratafy has a built-in review system. Use `list_reviews` and `get_latest_review` to reference past assessments and show progress over time.

## Insight Synthesis

Use `synthesize_insights` to find patterns across multiple insights. This is powerful for:

- Identifying recurring themes across coaching sessions
- Spotting strategic patterns that aren't obvious from individual data points
- Preparing synthesis reports for leadership

Use `get_insight_hierarchy` to see how insights relate to each other in a tree structure.

## When to Use This Knowledge

Activate this skill when:

- A user asks for a report, summary, or briefing
- Someone is preparing for a meeting, board session, or review
- A coach needs to present progress to a client
- The conversation involves communicating strategy to stakeholders
- Someone asks "how are we doing?" or "what's the status?"
