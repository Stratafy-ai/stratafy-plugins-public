---
description: Review team capacity and health against strategic demands
---

# /stratafy-expert-chro:team-health

Review team capacity and health against strategic demands. Surfaces where the team is stretched, where there are gaps, and what the strategy requires.

## When to Use

- Regular team health check-in
- Before headcount planning or restructuring
- When strategies feel under-resourced
- After a departure or team change

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "team-health"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]` — company context and strategic landscape
- `list_strategies` — active strategies and their leads
- `list_initiatives` — active initiatives and ownership
- `list_teams` — team structure
- `list_team_members` — who is on which team
- `list_key_priorities` — current priorities
- `list_values` — company values (culture alignment)

### Step 3: Assess

Analyse the data through a people lens:
- Are all active strategies adequately resourced with leads?
- Are there initiatives without clear ownership?
- Is work concentrated on too few people?
- Do the priorities require capabilities the team doesn't have?
- Are teams aligned to strategic priorities?
- Are there people on teams that don't map to any active strategy?

### Step 4: Present

```
TEAM HEALTH — [Workspace Name]
[Date]

━━━ CAPACITY ALIGNMENT ━━━━━━━━━━━━━━━━
[Which strategies are resourced vs under-resourced]

━━━ OWNERSHIP GAPS ━━━━━━━━━━━━━━━━━━━━
[Strategies or initiatives without clear leads]

━━━ CONCENTRATION RISK ━━━━━━━━━━━━━━━━
[Where too much depends on too few people]

━━━ CAPABILITY GAPS ━━━━━━━━━━━━━━━━━━━
[Strategic priorities that require skills the team may lack]

━━━ RECOMMENDATIONS ━━━━━━━━━━━━━━━━━━━
1. [Specific action with strategic rationale]
2. [Specific action with strategic rationale]
3. [Specific action with strategic rationale]
```

### Step 5: Offer Follow-ups

- "Want to plan a hire?" — `/stratafy-expert-chro:hiring-brief`
- "Want to review org structure?" — `/stratafy-expert-chro:org-review`
- "Want to check retention risks?" — `/stratafy-expert-chro:retention-scan`

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "team-health"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Strategy-linked.** Every finding connects to a strategy or initiative. "We need more engineers" becomes "The Product Architecture strategy has 5 active initiatives and 1 engineer."
2. **Quantify.** Numbers over adjectives. "Stretched" → "4 strategies, 2 people, 12 active initiatives."
3. **Concentration risk is a people risk.** Single points of failure are the most dangerous team health issue.
4. **Recommend, don't decide.** Surface the issue, propose the action, let the human commit.
5. When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
