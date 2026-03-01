# Stratafy Cowork Plugin

Claude Cowork plugin for strategy coaches and consultants.

## Repository Structure

The plugin is developed inside the main Stratafy monorepo at `cowork-plugin/` and published to a separate public repo:

| Location           | Repo                           | Purpose                       |
| ------------------ | ------------------------------ | ----------------------------- |
| `cowork-plugin/`   | `ljcremer/stratafy`            | Development (source of truth) |
| `stratafy-cowork/` | `Stratafy-ai/stratafy-plugins` | Published plugin (public)     |

The directory is renamed during publish: `cowork-plugin/` locally becomes `stratafy-cowork/` in the plugin repo.

## Versioning

Version is tracked in `cowork-plugin/.claude-plugin/plugin.json`:

```json
{
  "version": "1.1.0"
}
```

Follow semver:

- **Patch** (1.1.x) — fixes to existing commands or skills (typos, clarifications, bug fixes)
- **Minor** (1.x.0) — new skills, new commands, or significant updates to existing ones
- **Major** (x.0.0) — breaking changes (renamed commands, removed skills, restructured plugin)

Always bump the version before pushing to the plugin repo.

## Publishing

After making changes locally, sync to the plugin repo:

```bash
# 1. Clone the plugin repo
git clone https://github.com/Stratafy-ai/stratafy-plugins.git /tmp/stratafy-plugins

# 2. Sync local changes (rsync handles additions, updates, and deletions)
rsync -av --delete cowork-plugin/ /tmp/stratafy-plugins/stratafy-cowork/ --exclude='.git'

# 3. Commit and push
cd /tmp/stratafy-plugins
git add stratafy-cowork/
git commit -m "feat: describe what changed"
git push origin main

# 4. Clean up
rm -rf /tmp/stratafy-plugins
```

The `--delete` flag in rsync ensures removed files are also removed in the plugin repo. The `--exclude='.git'` prevents overwriting the plugin repo's git history.

## Plugin Contents

### Commands (6)

| Command                     | Description                                       |
| --------------------------- | ------------------------------------------------- |
| `/stratafy:setup-workspace` | Onboard a new client workspace from a company URL |
| `/stratafy:session-debrief` | Parse session notes into strategic artifacts      |
| `/stratafy:radar-scan`      | Run competitive/market intelligence scans         |
| `/stratafy:pulse`           | Quick strategic health check                      |
| `/stratafy:decision-brief`  | Prepare context for a pending decision            |
| `/stratafy:strategy-review` | Deep strategy review with alignment scans         |

### Skills (7)

| Skill                   | Description                                                       |
| ----------------------- | ----------------------------------------------------------------- |
| `strategy-foundations`  | First-principles strategy knowledge                               |
| `workspace-builder`     | End-to-end workspace creation flow                                |
| `signal-intelligence`   | Signal detection and radar operations                             |
| `execution-tracking`    | OKRs, initiatives, assumptions, risks                             |
| `coaching-facilitation` | Multi-client coaching workflows                                   |
| `strategic-reporting`   | Audience-specific reports and briefings                           |
| `strategy-documents`    | Create, organise, and format strategy documents and presentations |

### MCP Servers (6)

Configured in `.mcp.json`: Stratafy, Slack, Google Calendar, Gmail, Notion, Linear.
