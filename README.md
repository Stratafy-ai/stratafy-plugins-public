# Stratafy Plugins

Claude Cowork plugins for Stratafy — the operating system for strategy.

## Plugins

| Plugin | Who It's For | Description |
| --- | --- | --- |
| [`stratafy-cowork`](./stratafy-cowork/) | Strategy coaches & consultants | Workspace setup, session debriefs, strategy reviews, radar scans |
| [`stratafy-finance`](./stratafy-finance/) | Finance Directors & finance teams | COA design, financial alignment scans, budget mapping, investor prep |

## Installation

```bash
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-cowork
claude plugins add Stratafy-ai/stratafy-plugins/stratafy-finance
```

## Development

Plugins are developed inside the main Stratafy monorepo and synced here for publishing:

| Local (monorepo)    | Published (this repo)  |
| ------------------- | ---------------------- |
| `cowork-plugin/`    | `stratafy-cowork/`     |
| `finance-plugin/`   | `stratafy-finance/`    |

### Publishing Changes

```bash
# Clone this repo
git clone https://github.com/Stratafy-ai/stratafy-plugins.git /tmp/stratafy-plugins

# Sync whichever plugin changed
rsync -av --delete cowork-plugin/ /tmp/stratafy-plugins/stratafy-cowork/ --exclude='.git'
rsync -av --delete finance-plugin/ /tmp/stratafy-plugins/stratafy-finance/ --exclude='.git'

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
