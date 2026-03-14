# The Stratafy Team

A full fractional leadership team, powered by AI. Every role grounded in your live strategy.

## The Team

| Plugin | Who It's For | Description |
| --- | --- | --- |
| [`stratafy-guardian`](./stratafy-guardian/) | Strategy guardians, coaches & consultants | Workspace setup, session debriefs, strategy reviews, radar scans |
| [`stratafy-fd`](./stratafy-fd/) | Finance Directors & finance teams | Fractional Finance Director — COA design, financial alignment, budget mapping, investor prep |
| [`stratafy-team`](./stratafy-team/) | Every team member | Daily/weekly rhythm, strategic context, role-adapted coaching |
| [`stratafy-cos`](./stratafy-cos/) | Founders, CEOs, Chiefs of Staff | Fractional Chief of Staff — exec briefs, initiative tracking, alignment checks, decision logs |
| [`stratafy-cmo`](./stratafy-cmo/) | Marketing leaders & brand teams | Fractional CMO — brand audits, campaign briefs, positioning analysis |
| [`stratafy-cro`](./stratafy-cro/) | Revenue leaders & sales teams | Fractional CRO — pipeline reviews, deal strategy, revenue alignment |
| [`stratafy-cpo`](./stratafy-cpo/) | Product leaders & product teams | Fractional CPO — roadmap reviews, feature briefs, product-strategy alignment |
| [`stratafy-cto`](./stratafy-cto/) | Technical leaders & engineering teams | Fractional CTO — tech reviews, architecture briefs, technical debt assessment |
| [`stratafy-chro`](./stratafy-chro/) | People leaders & HR teams | Fractional CHRO — team health checks, hiring briefs, org-strategy alignment |
| [`stratafy-gc`](./stratafy-gc/) | Legal & compliance teams | Fractional General Counsel — risk reviews, compliance briefs, regulatory alignment |
| [`stratafy-advisor`](./stratafy-advisor/) | External startup advisors | Fractional Advisor — advisory sessions, board prep, stage-specific pattern recognition |

## Installation

```bash
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-guardian
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-fd
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-team
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-cos
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-cmo
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-cro
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-cpo
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-cto
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-chro
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-gc
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-advisor
```

## Development

Plugins are developed inside the main Stratafy monorepo and synced here for publishing:

| Local (monorepo)          | Published (this repo)        |
| ------------------------- | ---------------------------- |
| `guardian-plugin/`        | `stratafy-guardian/`         |
| `fd-plugin/`              | `stratafy-fd/`               |
| `team-plugin/`            | `stratafy-team/`             |
| `cos-plugin/`             | `stratafy-cos/`              |
| `cmo-plugin/`             | `stratafy-cmo/`              |
| `cro-plugin/`             | `stratafy-cro/`              |
| `cpo-plugin/`             | `stratafy-cpo/`              |
| `cto-plugin/`             | `stratafy-cto/`              |
| `chro-plugin/`            | `stratafy-chro/`             |
| `gc-plugin/`              | `stratafy-gc/`               |
| `advisor-plugin/`         | `stratafy-advisor/`          |

### Publishing Changes

```bash
# Clone this repo
git clone https://github.com/Stratafy-ai/stratafy-plugins.git /tmp/stratafy-plugins

# Sync whichever plugin changed
rsync -av --delete guardian-plugin/ /tmp/stratafy-plugins/stratafy-guardian/ --exclude='.git'
rsync -av --delete fd-plugin/ /tmp/stratafy-plugins/stratafy-fd/ --exclude='.git'

# Commit and push
cd /tmp/stratafy-plugins
git add .
git commit -m "feat: describe what changed"
git push origin main
```

### Versioning

Each plugin tracks its own version in `.claude-plugin/plugin.json`. Follow semver:

- **Patch** (x.x.1) — fixes to existing commands or skills
- **Minor** (x.1.0) — new skills, new commands, or significant updates
- **Major** (1.0.0) — breaking changes

Bump the version before pushing.

## MCP Connection

All plugins connect to Stratafy via MCP:

```json
{
  "mcpServers": {
    "stratafy": {
      "type": "http",
      "url": "https://app.stratafy.ai/api/mcp"
    }
  }
}
```

## License

Apache-2.0
