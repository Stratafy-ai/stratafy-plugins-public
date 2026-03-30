---
description: Assess whether the org structure supports the strategy
---

# /stratafy-expert-chro:org-review

Assess whether the organisation's structure supports its strategy. Surfaces structural misalignments, gaps, and recommendations for org design changes.

## When to Use

- Evaluating whether the org structure fits the strategy
- Before a restructure or reorg
- When new strategies are added and team alignment is unclear
- After significant team growth or contraction

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "org-review"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies"]` — company context and strategic landscape
- `get_strategy_tree` — full strategy hierarchy
- `list_initiatives` — active initiatives and ownership
- `get_organogram` — current org structure
- `list_teams` — team definitions and structure
- `list_team_members` — who sits where

### Step 3: Assess Structural Alignment

Analyse the org against the strategy:
- **Strategy coverage** — Does every active strategy have a team or person responsible?
- **Team-strategy mapping** — Do teams align to strategic priorities, or are they organised by function while strategy cuts across functions?
- **Structural gaps** — Are there strategies with no structural home?
- **Structural waste** — Are there teams or roles that don't map to any active strategy?
- **Span of control** — Are any managers overloaded or under-utilised?
- **Communication paths** — Does the org structure create unnecessary handoffs between teams that need to collaborate on the same strategy?

### Step 4: Present

```
ORG REVIEW — [Workspace Name]
[Date]

━━━ STRUCTURAL ALIGNMENT SCORE ━━━━━━━━
[X/10] — [one-line summary of alignment health]

━━━ STRATEGY COVERAGE ━━━━━━━━━━━━━━━━━
[Strategy 1]: [team/person responsible] — [aligned / gap / misaligned]
[Strategy 2]: [team/person responsible] — [aligned / gap / misaligned]
...

━━━ STRUCTURAL GAPS ━━━━━━━━━━━━━━━━━━━
[Strategies or initiatives with no structural home]

━━━ STRUCTURAL WASTE ━━━━━━━━━━━━━━━━━━
[Teams or roles not mapped to active strategy]

━━━ COMMUNICATION BOTTLENECKS ━━━━━━━━━
[Where the org structure creates friction for strategic execution]

━━━ RECOMMENDATIONS ━━━━━━━━━━━━━━━━━━━
1. [Specific structural change with strategic rationale]
2. [Specific structural change with strategic rationale]
3. [Specific structural change with strategic rationale]
```

### Step 5: Offer Follow-ups

- "Want to plan a restructure?" — deeper org design work
- "Want to check team health?" — `/stratafy-expert-chro:team-health`
- "Want to plan a hire to close a gap?" — `/stratafy-expert-chro:hiring-brief`

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "org-review"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Strategy drives structure.** The org should be shaped to execute the strategy, not the other way around.
2. **Quantify alignment.** "Misaligned" → "3 of 5 strategies have no dedicated team, and 2 teams don't map to any active strategy."
3. **No org chart for org chart's sake.** Every structural recommendation must connect to strategic execution.
4. **Be honest about trade-offs.** Functional orgs vs cross-functional pods both have costs — name them.
5. **Recommend, don't restructure.** Surface the findings and propose changes. The human decides.
6. When referencing strategies or teams, use markdown links with the `urls.detail` URL from tool responses.
