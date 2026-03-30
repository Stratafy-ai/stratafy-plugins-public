---
description: Scan for key person dependencies and retention risks
---

# /stratafy-expert-chro:retention-scan

Identify key person dependencies, single points of failure, and retention risks across the strategy. Surfaces where the loss of one person could stall strategic execution.

## When to Use

- Regular risk assessment for people dependencies
- After a departure or when someone signals they might leave
- Before headcount planning — understand what you can't afford to lose
- When a strategy feels fragile and you suspect it's a people issue

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "retention-scan"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Data

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]` — strategic landscape
- `list_strategies` — all active strategies and their leads
- `list_initiatives` — all active initiatives and their owners
- `list_teams` — team structure
- `list_team_members` — who is on which team

### Step 3: Assess Retention Risk

Analyse ownership concentration and dependencies:
- **Single points of failure** — People who are the sole lead on a strategy or sole owner of multiple initiatives
- **Key person dependencies** — Strategies that rely on one person's knowledge, relationships, or skills
- **Concentration clusters** — People carrying disproportionate load across strategies
- **Bus factor** — For each strategy, how many people would need to leave before it stalls?
- **Cross-strategy exposure** — People who are critical to multiple strategies simultaneously

### Step 4: Present

```
RETENTION SCAN — [Workspace Name]
[Date]

━━━ RISK HEAT MAP ━━━━━━━━━━━━━━━━━━━━━
🔴 Critical: [N] people are single points of failure
🟡 Elevated: [N] people carry disproportionate load
🟢 Healthy: [N] strategies have adequate coverage

━━━ CRITICAL DEPENDENCIES ━━━━━━━━━━━━━
[Person 1]:
  - Sole lead on: [Strategy A]
  - Sole owner of: [Initiative X], [Initiative Y], [Initiative Z]
  - Bus factor impact: [what stalls if they leave]

[Person 2]:
  - Sole lead on: [Strategy B], [Strategy C]
  - Bus factor impact: [what stalls if they leave]

━━━ STRATEGY BUS FACTOR ━━━━━━━━━━━━━━━
[Strategy 1]: Bus factor [N] — [assessment]
[Strategy 2]: Bus factor [N] — [assessment]
...

━━━ CONCENTRATION RISK ━━━━━━━━━━━━━━━━
[Person A]: [N] strategies, [N] initiatives — [risk level]
[Person B]: [N] strategies, [N] initiatives — [risk level]

━━━ RECOMMENDATIONS ━━━━━━━━━━━━━━━━━━━
1. [Specific action to reduce key person risk]
2. [Specific action to distribute ownership]
3. [Specific succession planning recommendation]
```

### Step 5: Offer Follow-ups

- "Want to plan succession for a key person?" — deeper succession planning
- "Want to hire to reduce concentration?" — `/stratafy-expert-chro:hiring-brief`
- "Want to review org structure?" — `/stratafy-expert-chro:org-review`

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "retention-scan"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Data-driven.** Every finding is backed by ownership data from the workspace, not speculation.
2. **Strategic framing.** "Person X is a risk" → "Person X is the sole lead on the GTM strategy with 6 active initiatives — their departure would stall 40% of our strategic execution."
3. **Actionable.** Every risk gets a mitigation recommendation: cross-train, document, hire backup, redistribute.
4. **No blame.** Concentration risk is a structural problem, not a performance issue. Frame it as org design, not individual failure.
5. **Honest about severity.** If the bus factor is 1 on a critical strategy, say so clearly.
6. When referencing strategies or people, use markdown links with the `urls.detail` URL from tool responses.
