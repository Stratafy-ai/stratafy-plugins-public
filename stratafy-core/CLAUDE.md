# Stratafy Core

Universal Stratafy connector. Links a Claude Desktop / Claude Code project folder to a Stratafy workspace, pulls that workspace's foundation and key context into the project, and keeps it in sync.

Workspace-agnostic by design: the workspace a project belongs to is **data in a file** (`<project-root>/.stratafy/link.json`), discovered and chosen once — never a hardcoded ID. Customer-branded surfaces (welcome, onboarding, help) live in sibling plugins; this connector is the platform layer underneath them.

## Commands

- `/stratafy:link` — Discover the workspaces you can access, pick one, link this project to it
- `/stratafy:sync` — Pull the linked workspace's foundation + key context into the project; refresh if stale
- `/stratafy:status` — Show the link, sync freshness, and what telemetry is reported

## Local Files

All inside the user's project folder so file tools can read them in Claude Code CLI and Claude Desktop / Cowork (Cowork only exposes the selected project folder). Resolve via `PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"`.

- `<project-root>/.stratafy/link.json` — project↔workspace binding (workspace id/name, linked_by, last_synced, sync_ttl_days)
- `<project-root>/.stratafy/foundation.md` — mission, vision, values, beliefs, principles (slow-changing)
- `<project-root>/.stratafy/context.md` — active strategies + key priorities (faster-changing)

The Write tool does not expand `~` — always use the absolute project-rooted path.

## Linking + sync model

1. `/stratafy:link` calls `select_workspace` with no ID → lists accessible workspaces → user picks → writes `link.json`
2. `/stratafy:sync` reads `link.json` → `get_workspace_snapshot(["foundation","strategies","key_priorities"])` → writes `foundation.md` + `context.md` → updates `last_synced`
3. Sync is TTL-based (default 7 days) + explicit `/stratafy:sync` + stale-auto-refresh when context is needed
4. If any command needs context and the project isn't linked, the link flow runs **inline** — never a hard "link first" failure

## Provenance

Every MCP call includes:

- `_source_plugin`: "stratafy-core"
- `_source_command`: the command being run (`link` / `sync` / `status`)
- `_change_reasoning`: brief explanation
- plus `_llm_model`, `_intent`, `_reason` (server-enforced)

## Privacy

Reports plugin lifecycle events (install, command-run counts, errors, uninstall) to Stratafy. Does NOT report conversation content, per-employee activity, or referenced files. `/stratafy:status` shows exactly what's transmitted, every time.
