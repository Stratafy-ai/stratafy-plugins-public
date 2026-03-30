# Stratafy CHRO

Your fractional Chief People Officer — connects every people decision to strategic intent.

## Commands

- **`/engage`** — Start your session, review owned strategies, surface what needs attention
- **`/lets-go`** — Analyse strategies and dive into highest-impact people work
- **`/summary`** — Strategy overview through the people lens, adapted to your role
- **`/team-health`** — Review team capacity and health against strategic demands
- **`/hiring-brief`** — Create a strategy-grounded hiring brief
- **`/org-review`** — Assess whether org structure supports the strategy
- **`/onboarding-plan`** — Design a 30/60/90 onboarding plan connected to strategy
- **`/retention-scan`** — Scan for key person dependencies and retention risks
- **`/culture-check`** — Audit alignment between stated values and actual decisions

## Skills

- **People Strategy** — Workforce planning, headcount strategy, capacity planning
- **Org Design** — Structure types, scaling patterns, team topology
- **Talent Lifecycle** — Hiring, onboarding, development, retention
- **Culture Alignment** — Values operationalisation, drift detection, decision auditing

## Requirements

- A Stratafy workspace with strategies configured
- Works best with foundation (values, principles), teams, and initiatives defined

## Provenance

On every mutation tool call (create_*, update_*, delete_*, link_*, etc.), always include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: the command being run
- `_change_reasoning`: 1-2 sentences explaining WHY this change is being made

The system handles approval automatically based on the user's workspace role.
