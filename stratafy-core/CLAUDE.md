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
- `<project-root>/CLAUDE.local.md` — managed sentinel block that `@`-imports the two cache files (the delivery mechanism)

The Write tool does not expand `~` — always use the absolute project-rooted path. The plugin owns only `CLAUDE.local.md`; it NEVER writes `CLAUDE.md` (that's the user's Folder-instructions surface in Cowork).

## Auto-grounding mechanism

Writing the `.stratafy/*.md` cache is necessary but not sufficient — nothing pulls it into a fresh session. The delivery is a managed, sentinel-fenced block in `CLAUDE.local.md`:

```markdown
<!-- stratafy:begin — managed by stratafy-core; do not edit inside this block -->
## Stratafy workspace binding
This project is bound to the **{{workspace}}** Stratafy workspace
(`workspace_id: {{workspace_id}}`).
Working rule: for ANY Stratafy MCP operation in this folder, pin this
workspace first — get_user_context(workspace_id: "{{workspace_id}}", …).
@.stratafy/foundation.md
@.stratafy/context.md
<!-- stratafy:end -->
```

The block does two jobs: `@`-imports give ambient *read* grounding; the embedded `workspace_id` + working rule make any Stratafy MCP *operation* in the folder target the bound workspace (operationalising the MCP contract's "select_workspace first, always; never assume a prior session"). `{{workspace_id}}` is data sourced from `link.json` (canonical) and re-asserted every sync — NOT a plugin-hardcoded ID.

Verified in Cowork: `CLAUDE.local.md` auto-loads every session and `@path` imports inline the file content with no tool call — so foundation + context are ambient grounding for free, every session, with zero ceremony. `CLAUDE.local.md` is conventionally gitignored (per-clone) and invisible to Cowork's Folder-instructions modal, so the plugin and the user never collide. The block is timestamp-free (volatile `last_synced` stays in `link.json`) to avoid per-session churn. Sync re-asserts the block idempotently, replacing only between the sentinels.

In git repos the link/sync flow adds `.stratafy/` and `CLAUDE.local.md` to `.gitignore`.

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
