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
- Re-assert the managed grounding block in `$PROJECT_ROOT/CLAUDE.local.md` (see next section)

Absolute paths only. The Write tool does not expand `~`.

## Grounding block — CLAUDE.local.md (the delivery mechanism)

Writing `.stratafy/*.md` is not enough on its own — nothing pulls them into a fresh session. The delivery mechanism is a **managed, sentinel-fenced block in `<project-root>/CLAUDE.local.md`** that `@`-imports the cache files. Confirmed in Cowork: `CLAUDE.local.md` auto-loads every session, and `@path` imports inline the referenced file's content with no tool call — so the foundation + context become ambient grounding for free, every session.

**Why `CLAUDE.local.md`, not `CLAUDE.md`:** `CLAUDE.md` is the user's own surface (Cowork's "Folder instructions" modal edits it). The plugin must never write there. `CLAUDE.local.md` also auto-loads, is conventionally gitignored (per-clone), and is invisible to the Folder-instructions modal — clean ownership separation.

**The managed block (exact shape; timestamp-free — volatile state stays in `link.json`):**

```markdown
<!-- stratafy:begin — managed by stratafy-core; do not edit inside this block. Refresh with /stratafy:sync -->
## Stratafy workspace grounding

This project is linked to the **{{workspace_name}}** Stratafy workspace.

The workspace foundation and active strategy context are imported below so they
load automatically at the start of every session:

@.stratafy/foundation.md
@.stratafy/context.md

If these are missing or you suspect they're stale, run `/stratafy:sync`.
Link + sync metadata lives in `.stratafy/link.json`.
<!-- stratafy:end -->
```

**Idempotent merge — never clobber the user's own content:**

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
LOCAL="$PROJECT_ROOT/CLAUDE.local.md"
```

- `CLAUDE.local.md` absent → create it with a minimal header + the managed block
- present, contains a `stratafy:begin`/`stratafy:end` pair → replace ONLY the text between (and including) the sentinels; leave everything else byte-for-byte
- present, no sentinels → append the managed block after existing content (preserve all of it)

Use Read then Write with surgical sentinel replacement. NEVER regenerate the whole file from scratch when sentinels already exist — the user may have written their own notes around the block.

**Gitignore (repos only):** if `$PROJECT_ROOT/.git` exists, ensure `.gitignore` contains `.stratafy/` and `CLAUDE.local.md` (append if missing; don't duplicate). This keeps the per-clone binding out of version control. If there's no `.git`, skip silently — not every project folder is a repo.

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
- Writes `<project-root>/.stratafy/foundation.md`, `<project-root>/.stratafy/context.md`, the `last_synced` field of `link.json`, and the managed sentinel block in `<project-root>/CLAUDE.local.md`
- May append to `<project-root>/.gitignore` (only if `.git` present)

## Rules

1. NEVER fetch without a link — run workspace-link inline if `link.json` is absent, then continue
2. NEVER call `get_workspace_snapshot` without `sections`
3. NEVER fabricate foundation/context — sparse state is surfaced honestly
4. NEVER write to `~/.stratafy/` — always the absolute project-rooted path
5. NEVER write to `CLAUDE.md` — that's the user's surface; the plugin owns only `CLAUDE.local.md`
6. NEVER regenerate `CLAUDE.local.md` wholesale when sentinels exist — replace only between the markers, preserve the rest
7. NEVER put a timestamp in the managed block — volatile state lives in `link.json` only (avoids per-session churn)
8. ALWAYS update `last_synced` after a successful write, preserving other `link.json` fields
9. ALWAYS re-assert the managed block on every sync (idempotent) so a fresh `@`-import is in place
10. Respect the TTL — fresh cache is served from disk unless the user forces a refresh
