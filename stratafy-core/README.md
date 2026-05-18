# stratafy-core

Universal Stratafy connector. Links a Claude Desktop / Claude Code project to a Stratafy workspace, downloads that workspace's foundation and key context into the project folder, and keeps it in sync.

Workspace-agnostic: the linked workspace is stored in `<project-root>/.stratafy/link.json`, discovered and chosen by the user — no hardcoded IDs. Customer-branded surfaces (welcome, onboarding, help) live in sibling plugins; this is the platform layer they sit on.

## What it does

- `/stratafy:link` — List the workspaces you can access, pick one, link this project to it
- `/stratafy:sync` — Pull the linked workspace's foundation + key context locally; refresh when stale
- `/stratafy:status` — Show the link, sync freshness, and telemetry transparency

## How linking works

`/stratafy:link` calls the MCP to list the workspaces your signed-in identity can access, you pick one, and the binding is written to `<project-root>/.stratafy/link.json`. Switching projects means a different (or no) link — the binding is per project folder.

## How sync works

`/stratafy:sync` reads the link, fetches the workspace's foundation (mission, vision, values, beliefs, principles) and key context (active strategies + key priorities) in a single snapshot call, and writes them to `.stratafy/foundation.md` and `.stratafy/context.md`. Sync is TTL-based (default 7 days), forceable via `/stratafy:sync`, and auto-refreshes when stale context is needed. If a command needs context and the project isn't linked yet, the link flow runs inline — no dead-end "link first" error.

## Local files

All inside the project folder (visible to file tools in both Claude Code CLI and Claude Desktop / Cowork):

- `<project-root>/.stratafy/link.json` — project↔workspace binding
- `<project-root>/.stratafy/foundation.md` — foundation document
- `<project-root>/.stratafy/context.md` — active strategies + key priorities

## Provenance

Every MCP call includes `_source_plugin: "stratafy-core"`, the command name, `_change_reasoning`, plus `_llm_model` / `_intent` / `_reason` (server-enforced).

## Privacy

Reports plugin lifecycle events (install, command-run counts, errors, uninstall) to Stratafy. Does NOT report conversation content, per-employee activity, or referenced files. `/stratafy:status` shows exactly what's transmitted.
