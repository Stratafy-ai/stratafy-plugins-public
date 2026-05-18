---
name: "workspace-sync"
description: "Pulls the linked Stratafy workspace's foundation and key context into the project folder and keeps it fresh on a TTL. Use when the user runs /stratafy:sync, when foundation/strategy/key-priority context is relevant to a request and the local cache is missing or stale, or when another stratafy-core operation needs grounded workspace context. Reads <project-root>/.stratafy/link.json for the workspace; if absent, runs the workspace-link flow inline first."
---

# Workspace Sync

Owns the local cache of the linked workspace's context. Generic: the workspace is whatever `<project-root>/.stratafy/link.json` points at — no hardcoded ID.

Two files, split by change cadence:

- `<project-root>/.stratafy/foundation.md` — mission, vision, values, beliefs, principles. Slow-changing.
- `<project-root>/.stratafy/context.md` — active strategies + key priorities. Faster-changing.

Both live inside the user's project folder so file tools can read them in Claude Code CLI and Claude Desktop / Cowork (Cowork only exposes the selected project folder).

## Trigger

Use when:

- The user runs `/stratafy:sync`
- Foundation / strategy / key-priority context is relevant to a request and `.stratafy/foundation.md` or `.stratafy/context.md` is missing or older than the TTL
- Another stratafy-core skill needs grounded workspace context

## Read the link first

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
LINK="$PROJECT_ROOT/.stratafy/link.json"
test -f "$LINK" && cat "$LINK" || echo "UNLINKED"
```

`UNLINKED` → run the **workspace-link** skill inline (discover, pick, write `link.json`), then continue. Never fail with "link first".

From `link.json`: `workspace_id`, `workspace_name`, `last_synced`, `sync_ttl_days` (default 7 if absent).

## Cache logic

- `foundation.md` / `context.md` missing → fetch
- present AND `now - last_synced < sync_ttl_days` → fresh; serve from cache, show freshness line
- present AND `now - last_synced ≥ sync_ttl_days` → stale; fetch
- user explicitly forces a refresh → fetch regardless

## Fetch

`get_user_context(workspace_id: <from link.json>, command_name: "sync", plugin_name: "stratafy-core", …provenance)` — validate + pin + identity in one call. Do NOT call `select_workspace` separately.

`get_workspace_snapshot(sections: ["foundation", "strategies", "key_priorities"], …provenance)` — all three in one call. ALWAYS pass `sections`.

Provenance: `_llm_model`, `_intent: "user_request"`, `_reason`, `_source_plugin: "stratafy-core"`, `_source_command: "sync"`.

## Write

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
mkdir -p "$PROJECT_ROOT/.stratafy"
```

- Assemble the foundation portion → `$PROJECT_ROOT/.stratafy/foundation.md` (canonical document format)
- Assemble strategies + key priorities → `$PROJECT_ROOT/.stratafy/context.md`
- Update `link.json` `last_synced` to ISO8601 now; preserve every other field exactly

Absolute paths only. The Write tool does not expand `~`.

## Honest empty state

If foundation or context is sparse/empty, surface it — never fabricate to fill gaps:

> *"{{workspace_name}}'s foundation in Stratafy is still being populated — some sections are in draft."*

## Freshness line

Every display includes it:

> *Last synced {{timestamp}}. Auto-refreshes after {{sync_ttl_days}} days. Force: /stratafy:sync (say "refresh").*

## Per-project caches

Each project folder has its own `.stratafy/`. A user with two projects linked to two workspaces sees two independent caches — correct semantics. Cross-project shared state would require a live MCP fetch each time.

## Provenance

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"sync"`
- `_change_reasoning`: e.g., `"{{n}}-day TTL expiry for {{workspace_name}}"`

## Local Files

- Reads `<project-root>/.stratafy/link.json`
- Writes `<project-root>/.stratafy/foundation.md`, `<project-root>/.stratafy/context.md`, and the `last_synced` field of `link.json`

## Rules

1. NEVER fetch without a link — run workspace-link inline if `link.json` is absent, then continue
2. NEVER call `get_workspace_snapshot` without `sections`
3. NEVER fabricate foundation/context — sparse state is surfaced honestly
4. NEVER write to `~/.stratafy/` — always the absolute project-rooted path
5. ALWAYS update `last_synced` after a successful write, preserving other `link.json` fields
6. Respect the TTL — fresh cache is served from disk unless the user forces a refresh
