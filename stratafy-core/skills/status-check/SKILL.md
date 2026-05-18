---
name: "status-check"
description: "Reports which Stratafy workspace this project is linked to, how fresh the synced context is, and what telemetry the plugin reports to the workspace admin. Generic — reads <project-root>/.stratafy/link.json, never hardcodes a workspace. Use when the user runs /stratafy:status or asks about sync state, the link, connection, what the plugin knows, or what data is transmitted."
---

# Status Check

Reports the project↔workspace link, sync freshness, and telemetry transparency. Workspace-agnostic: the link is read from `<project-root>/.stratafy/link.json`.

## Trigger

Use when:

- User runs `/stratafy:status`
- User asks about sync state, the link, connection state, what data is transmitted
- User expresses concern about privacy or what the plugin "sees"

## Resolving the project root

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
```

Claude Code CLI: `$CLAUDE_PROJECT_DIR` else `pwd`. Claude Desktop / Cowork: the selected project folder (the only place file tools can read).

## Read the link

```bash
LINK="$PROJECT_ROOT/.stratafy/link.json"
test -f "$LINK" && cat "$LINK" || echo "UNLINKED"
```

- `UNLINKED` → render the unlinked state and offer to link inline (hand to workspace-link). This is a valid state, not an error.
- Linked → read `workspace_name`, `workspace_id`, `linked_by`, `linked_at`, `last_synced`, `sync_ttl_days`

## Validate (linked only)

`get_user_context(workspace_id: <from link.json>, command_name: "status", plugin_name: "stratafy-core", …provenance)` — confirms the link still resolves and the identity still has access. Do NOT call `select_workspace` separately. If it fails → render the connection-issue block, never a healthy one.

## Cache freshness

```bash
ls -la "$PROJECT_ROOT/.stratafy/foundation.md" 2>/dev/null
ls -la "$PROJECT_ROOT/.stratafy/context.md" 2>/dev/null
```

Days since `last_synced`; stale if `≥ sync_ttl_days` (default 7).

## Status block

**Linked:** connection ✓, linked workspace name, sync age + fresh/stale, force-refresh hint (`/stratafy:sync`), re-link hint (`/stratafy:link`), plugin version, then the telemetry transparency block.

**Unlinked:** "not linked yet" + `/stratafy:link` hint + plugin version + the telemetry block (framed as "once linked"). Then offer to link inline.

## Telemetry transparency

This block is non-negotiable and always present, linked or not:

```
What this plugin reports to your workspace admin:
- Plugin install / activation / uninstall events
- Command-run counts per plugin
- Error counts when commands fail

What this plugin does NOT report:
- Your conversation content
- Per-employee activity
- Files you reference
```

The trust posture depends on this being honest and visible every time.

## Failure modes

Pin/validate fails, or the linked workspace no longer resolves:

```
✗ Could not reach the linked workspace ({{workspace_name}})
  Error: {{message}}

  Try: /stratafy:status (retry)
  Or:  /stratafy:link (re-link if the workspace changed)
```

Do NOT pretend everything is fine.

## Provenance

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"status"`

## Local Files

- Reads `<project-root>/.stratafy/link.json`, and the mtimes of `foundation.md` / `context.md`

## Rules

1. NEVER hardcode a workspace — read it from `.stratafy/link.json`
2. ALWAYS show the telemetry transparency block, linked or unlinked
3. ALWAYS surface auth/connection issues with recovery steps — never fake a healthy state
4. Unlinked is a valid state — render cleanly, offer to link inline
