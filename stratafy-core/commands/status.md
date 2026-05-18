---
description: "Show which Stratafy workspace this project is linked to, how fresh the synced context is, and what telemetry the plugin reports"
---

# /stratafy:status

Show the project↔workspace link, sync freshness, and telemetry transparency.

## Process

### Step 1: Resolve project root + read the link

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
LINK="$PROJECT_ROOT/.stratafy/link.json"
test -f "$LINK" && cat "$LINK" || echo "UNLINKED"
```

- `UNLINKED` → render the unlinked state (Step 4), offer to link inline. Do NOT hardcode a workspace.
- Link present → read `workspace_name`, `workspace_id`, `linked_by`, `last_synced`, `sync_ttl_days`

### Step 2: Validate the connection (only if linked)

`get_user_context(workspace_id: <from link.json>, command_name: "status", plugin_name: "stratafy-core", …provenance)` — confirms the link still resolves and the identity still has access. Provenance: `_llm_model`, `_intent: "user_request"`, `_reason`, `_source_plugin: "stratafy-core"`, `_source_command: "status"`.

If it fails → Step 5 (connection issue).

### Step 3: Check cache freshness

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
ls -la "$PROJECT_ROOT/.stratafy/foundation.md" 2>/dev/null
ls -la "$PROJECT_ROOT/.stratafy/context.md" 2>/dev/null
grep -q 'stratafy:begin' "$PROJECT_ROOT/CLAUDE.local.md" 2>/dev/null && echo "GROUNDING_BLOCK_PRESENT" || echo "GROUNDING_BLOCK_MISSING"
```

Compute days since `last_synced`. Stale if `≥ sync_ttl_days`. The grounding-block check verifies the auto-load wiring is actually in place — cache files without the `CLAUDE.local.md` block means context exists on disk but won't reach a fresh session.

### Step 4: Render the status block

**Linked:**

```
Stratafy plugin status:

✓ Connected as {{user.email}}
✓ Project linked to: {{workspace_name}}
✓ Foundation + context synced {{n}} days ago
  {{"fresh — next auto-refresh in " (ttl-n) " days" | "STALE — run /stratafy:sync"}}
{{✓ Auto-grounding wired (CLAUDE.local.md imports the cache — loads every session)
  | ✗ Auto-grounding NOT wired — run /stratafy:sync to install the CLAUDE.local.md block}}
  Force refresh: /stratafy:sync

Linked by {{linked_by}} on {{linked_at}}
Plugin version: stratafy-core {{version}}

What this plugin reports to your workspace admin:
- Plugin install / activation / uninstall events
- Command-run counts per plugin
- Error counts when commands fail

What this plugin does NOT report:
- Your conversation content
- Per-employee activity
- Files you reference

Re-link: /stratafy:link
```

**Unlinked:**

```
Stratafy plugin status:

○ This project is not linked to a Stratafy workspace yet.

Link it: /stratafy:link
  (lists the workspaces you can access; pick one)

Plugin version: stratafy-core {{version}}

What this plugin reports to your workspace admin (once linked):
- Plugin install / activation / uninstall events
- Command-run counts per plugin
- Error counts when commands fail

What this plugin does NOT report:
- Your conversation content
- Per-employee activity
- Files you reference
```

Then offer: *"Want me to link it now?"* — if yes, run the workspace-link flow inline.

### Step 5: Surface connection issues

If the pin/validate call fails (auth, network, missing MCP config, or the linked workspace no longer resolves):

```
✗ Could not reach the linked workspace ({{workspace_name}})
  Error: {{message}}

  Try: /stratafy:status (retry)
  Or:  /stratafy:link (re-link if the workspace changed)
```

Do NOT pretend everything is fine.

## Provenance Context

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"status"`

## Rules

1. NEVER hardcode a workspace — the linked workspace comes from `.stratafy/link.json`
2. ALWAYS show the telemetry transparency block (linked or unlinked) — non-negotiable for the trust posture
3. ALWAYS surface auth/connection issues with concrete recovery steps — never fake a healthy state
4. Unlinked is a valid state, not an error — render it cleanly and offer to link
