---
name: strategy-review
description: Run a deep analysis of a strategy branch including alignment, coherence, execution health, risks, and assumptions
---

# /stratafy:strategy-review

Run a deep analysis of a strategy branch — alignment, coherence, execution health, risks, assumptions, and signals. Produces a comprehensive review suitable for leadership meetings, board presentations, or coaching sessions.

## When to Use

- Quarterly strategy reviews
- Preparing for a board meeting
- Coaching session focused on strategic health
- After a major market shift or internal change
- Annual strategic planning

## Input

Provide one of:
- **A specific strategy name or area** — "review our Product strategy"
- **Nothing** — I'll review the full strategy tree starting from the top
- **A specific question** — "review whether our GTM strategy is still aligned with our vision"

## Process

### Step 1: Load the Strategy Tree
I'll use `get_strategy_tree` to get the full hierarchy of the strategy being reviewed, including sub-strategies, initiatives, and objectives.

### Step 2: Run Alignment Scans
- `run_alignment_foundation_scan` — Are strategies aligned with each other and the foundation?
- `run_alignment_coherence_scan` — Is the strategy internally consistent? Do the pieces fit together logically?

### Step 3: Assess Execution
For each strategy in the branch:
- Review linked initiatives — status, progress, timeline
- Check OKR scores — on track, at risk, off track
- Review metrics — trending positively or negatively?

### Step 4: Review Intelligence Layer
- `get_assumptions_for_context` — What assumptions underpin this strategy? Are they still valid?
- `get_risks_for_context` — What risks threaten this strategy? Have any escalated?
- `get_signals_for_strategy` — What signals are relevant? Any unprocessed?
- `list_decisions` — What decisions have been made? What's pending?

### Step 5: Synthesise Insights
- `search_insights` — What insights relate to this strategy area?
- Look for patterns, contradictions, or gaps

### Step 6: Generate Review

**Strategy Overview**
- Name, type, status, time horizon
- Description and current strategic intent
- Connection to parent strategy and foundation

**Alignment Assessment**
- Internal alignment score with commentary
- Coherence score with specific areas of concern
- Foundation alignment — does this strategy serve the mission and vision?

**Execution Scorecard**
- Initiative progress — completed, on track, at risk, overdue
- OKR scorecard — scores by objective with trend
- Key metrics summary

**Intelligence Summary**
- Assumption health — validated vs. invalidated vs. untested
- Risk exposure — top risks with mitigation status
- Signal watch — relevant signals and their implications
- Active decisions pending resolution

**Strategic Assessment**
- Strengths — what's working well
- Concerns — what needs attention
- Gaps — what's missing or under-resourced
- Opportunities — potential strategic moves

**Recommendations**
- Specific, actionable recommendations ranked by priority
- Suggested timeline for implementation
- Who should own each recommendation

## Output

A structured review document that can be:
- Presented in a leadership meeting
- Shared with a coaching client
- Used as the basis for a board briefing
- The review output can be saved as a document using `create_document`

## For Coaches
Frame the review in terms your client understands:
- Use their methodology language (Rocks, BHAG, V/TO, etc.)
- Focus on progress since the last session
- Highlight where the coach can add the most value next session
- Include preparation notes for the client
