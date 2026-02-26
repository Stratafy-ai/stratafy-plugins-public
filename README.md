# Stratafy Plugins for Claude

Strategic intelligence plugins for Claude Code and Claude Cowork — powered by [Stratafy](https://stratafy.ai), the operating system for strategy.

## Available Plugins

### stratafy-cowork

Turn strategy sessions into living strategic workspaces. Designed for strategy coaches, founders, and leadership teams.

**6 Skills** (auto-activated by Claude when relevant):
- `strategy-foundations` — What strategy is, how Stratafy workspaces are structured
- `workspace-builder` — Creating and populating workspaces from session notes or methodology templates
- `coaching-facilitation` — Session facilitation, multi-client management, debrief techniques
- `execution-tracking` — OKRs, initiatives, risks, assumptions, review cycles
- `signal-intelligence` — Signal pipeline, radar operations, competitive intelligence
- `strategic-reporting` — Reports, board prep, stakeholder communication

**6 Commands** (invoke explicitly):
- `/stratafy-cowork:setup-workspace` — Guided workspace creation and population
- `/stratafy-cowork:session-debrief` — Parse session notes into strategic artifacts
- `/stratafy-cowork:pulse` — Quick strategic health check
- `/stratafy-cowork:strategy-review` — Deep strategy analysis with alignment scoring
- `/stratafy-cowork:radar-scan` — Competitive and market intelligence scan
- `/stratafy-cowork:decision-brief` — Prepare decision context for stakeholders

**Connectors**: Stratafy MCP (169 tools), Slack, Google Calendar, Gmail, Notion, Linear

## Installation

```bash
# Add the Stratafy marketplace
/plugin marketplace add Stratafy-ai/stratafy-plugins

# Install the Cowork plugin
/plugin install stratafy-cowork@stratafy
```

## Setup

After installing, configure your Stratafy connection:

1. Get an API key from your [Stratafy workspace](https://app.stratafy.ai)
2. When you first use a Stratafy command, Claude will prompt you to connect

## Who This Is For

- **Strategy coaches** — Set up client workspaces from session notes, debrief efficiently, track progress across clients
- **Founders & CEOs** — Manage your strategic workspace, run pulse checks, prepare board materials
- **Strategy teams** — Track execution, run alignment scans, process signals
- **Advisors & board members** — Get decision briefs with full strategic context

## Learn More

- [Stratafy](https://stratafy.ai) — The operating system for strategy
- [Stratafy MCP Documentation](https://stratafy.ai/docs/mcp) — 169 strategic intelligence tools

## License

Apache-2.0
