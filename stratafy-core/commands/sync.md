---
description: "Pull this project's linked Stratafy workspace foundation and key context into the project folder, and refresh it if stale"
---

# /stratafy:sync

Download the linked workspace's foundation (mission, vision, values, beliefs, principles) and key context (active strategies + key priorities) into the project folder, so Claude is grounded in this workspace's actual strategy — and keep it fresh.

## Process

### Step 1: Resolve project root + read the link

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
LINK="$PROJECT_ROOT/.stratafy/link.json"
test -f "$LINK" && cat "$LINK" || echo "UNLINKED"
```

- `UNLINKED` → this project isn't linked yet. Do NOT fail. Run the **workspace-link** flow inline (`/stratafy:link` behaviour) — discover workspaces, let the user pick, write `link.json` — then continue here.
- Link present → read `workspace_id`, `workspace_name`, `last_synced`, `sync_ttl_days`

### Step 2: Decide fetch vs cache

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
ls -la "$PROJECT_ROOT/.stratafy/foundation.md" 2>/dev/null
ls -la "$PROJECT_ROOT/.stratafy/context.md" 2>/dev/null
```

- Files missing → fetch (Step 3)
- Files present AND `now - last_synced < sync_ttl_days` → already fresh; display the freshness line, stop. (Unless the user explicitly asked to force a refresh.)
- Files present AND `now - last_synced ≥ sync_ttl_days` → stale; fetch (Step 3)

User can force a refresh by running `/stratafy:sync` and saying "refresh", or by deleting the cache files.

### Step 3: Pin + fetch

One pin-and-context call, then one snapshot call.

`get_user_context(workspace_id: <from link.json>, command_name: "sync", plugin_name: "stratafy-core", …provenance)` — validates access, pins the session, returns identity.

`get_workspace_snapshot(sections: ["foundation", "strategies", "key_priorities"], …provenance)` — returns all three in one call. ALWAYS pass `sections` — never call without it (the full payload overflows context).

Provenance on both: `_llm_model`, `_intent: "user_request"`, `_reason`, `_source_plugin: "stratafy-core"`, `_source_command: "sync"`.

### Step 4: Write the local context

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
mkdir -p "$PROJECT_ROOT/.stratafy"
```

Write two files (absolute paths):

- `$PROJECT_ROOT/.stratafy/foundation.md` — the foundation portion: mission, vision, values, beliefs, principles, in canonical document format
- `$PROJECT_ROOT/.stratafy/context.md` — the key context: active strategies (name + one-line) and active key priorities (title + what each bundles)

Then update `link.json` `last_synced` to the current ISO8601 timestamp (preserve all other fields).

### Step 5: Display

Render a short summary, always including the freshness line:

```
Synced {{workspace_name}} from Stratafy.

Foundation: {{mission one-liner}} · {{n}} values · {{n}} beliefs · {{n}} principles
Context:    {{n}} active strategies · {{n}} key priorities

Last synced: {{timestamp}}. Auto-refreshes after {{sync_ttl_days}} days.
Force refresh: /stratafy:sync (say "refresh")
```

### Honest empty state

If foundation or context is empty/sparse, say so — never fabricate:

> *"{{workspace_name}}'s foundation in Stratafy is still being populated — some sections are in draft."*

## Provenance Context

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"sync"`
- `_change_reasoning`: e.g., `"Cache miss / {{n}}-day TTL expiry for workspace {{name}}"`

## Rules

1. NEVER fetch without a link — if unlinked, run the workspace-link flow inline first, then continue
2. NEVER call `get_workspace_snapshot` without `sections` — the full payload overflows context
3. NEVER fabricate foundation/context to fill gaps — surface sparse state honestly
4. NEVER write to `~/.stratafy/` — always the absolute project-rooted path
5. ALWAYS update `link.json` `last_synced` after a successful write, preserving other fields
6. Respect the TTL — don't re-fetch fresh cache unless the user explicitly forces a refresh
