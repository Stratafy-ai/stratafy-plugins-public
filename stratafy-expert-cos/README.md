# Stratafy CoS

Your AI Chief of Staff — the cross-functional connective tissue that keeps leadership teams aligned, informed, and decisive.

## What This Plugin Does

The Chief of Staff sees across all teams, strategies, and initiatives. It surfaces what leadership needs to know, facilitates decisions, tracks follow-through, and maintains the operational rhythm — so leaders can focus on leading, not chasing updates.

## Commands

### Executive Intelligence
- **`/daily-brief`** — Morning strategic briefing: yesterday's provenance digest, what needs attention, today's focus, blind spots
- **`/exec-brief`** — On-demand executive briefing: strategic health, key metrics, what needs attention, wins, upcoming
- **`/weekly-update`** — Structured weekly leadership update: scorecard, wins, concerns, decisions, next week's focus

### Portfolio Management
- **`/initiative-tracker`** — Track all active initiatives: what's on track, what's slipping, resource conflicts, strategic gaps

### Alignment & Governance
- **`/alignment-check`** — Cross-functional alignment scan: strategy conflicts, gaps, tensions, dependencies
- **`/decision-log`** — Capture, facilitate, review, and track strategic decisions with full context and follow-ups

### Meeting Support
- **`/meeting-prep`** — Tailored briefing packs for board meetings, exec standups, investor calls, all-hands, or 1:1s

## Skills

- **Cross-Functional Alignment** — See the organisation as a system; spot conflicts, gaps, and dependencies across functions
- **Executive Communication** — BLUF, pyramid structure, traffic-light everything, quantify or don't mention
- **Initiative Portfolio** — Portfolio management, not list management: balance, trade-offs, health signals
- **Decision Intelligence** — Decision types, quality frameworks, tracking, and anti-patterns (debt, amnesia, bottlenecks)
- **Operational Rhythm** — Establish and maintain the operating cadence from daily to annual

## Requirements

- A Stratafy workspace with strategies, metrics, priorities, and initiatives configured
- Linear integration for execution tracking (optional but recommended)
- Slack integration for communication context (optional)
- Google Calendar for meeting awareness (optional)

## Setup

1. Install the plugin in Claude
2. Connect your Stratafy workspace via MCP
3. Connect Linear, Slack, and Google Calendar via MCP (optional)
4. Start with `/daily-brief` each morning, or `/exec-brief` for on-demand intelligence

## Who It's For

- **Founders/CEOs** who need to see the full picture without reading 10 reports
- **Chiefs of Staff** who want an AI partner to scale their cross-functional work
- **COOs** managing operational excellence and strategic execution
- **Leadership teams** who want alignment without more meetings

## Provenance

On every mutation tool call (create_*, update_*, delete_*, link_*, etc.), always include:
- `_source_plugin`: "stratafy-expert-cos"
- `_source_command`: the command being run
- `_change_reasoning`: 1-2 sentences explaining WHY this change is being made

The system handles approval automatically based on the user's workspace role.
