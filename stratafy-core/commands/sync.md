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

One pin call, then two scoped fetches.

`get_user_context(workspace_id: <from link.json>, command_name: "sync", plugin_name: "stratafy-core", …provenance)` — validates access, pins the session, returns identity.

`get_workspace_snapshot(sections: ["foundation", "strategies", "key_priorities"], …provenance)` — foundation + key context in one call. ALWAYS pass `sections` — never call without it (the full payload overflows context).

`get_user_strategy(…provenance)` — the caller's **team-scoped strategic loadout**: active objectives, the metrics behind them (current value + green/yellow/red band), and recent decisions. Caller-scoped — no parameters needed. Returns `scope: "team"` when the user has a team, `"role"` when it falls back to the strategies they personally lead, `"empty"` when there is nothing to surface.

Provenance on all three: `_llm_model`, `_intent: "user_request"`, `_reason`, `_source_plugin: "stratafy-core"`, `_source_command: "sync"`.

### Step 4: Write the local context

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
mkdir -p "$PROJECT_ROOT/.stratafy"
```

Write the cache files (absolute paths):

- `$PROJECT_ROOT/.stratafy/foundation.md` — the foundation portion: mission, vision, values, beliefs, principles, in canonical document format
- `$PROJECT_ROOT/.stratafy/context.md` — the key context: active strategies (name + one-line) and active key priorities (title + what each bundles)
- `$PROJECT_ROOT/.stratafy/loadout.md` — the team-scoped strategic loadout from `get_user_strategy`: team name, top active objectives (name · target · date · priority), the metrics behind them (current value + RAG band), and 2-3 recent decisions. If `scope` is `"role"`, note it is a fallback (strategies the user leads, no team); if `"empty"`, write a one-line honest note rather than an empty document.

Then update `link.json` `last_synced` to the current ISO8601 timestamp (preserve all other fields).

### Step 4b: Re-assert the grounding block in CLAUDE.local.md

The cache files do nothing on their own — the delivery mechanism is a managed, sentinel-fenced block in `$PROJECT_ROOT/CLAUDE.local.md` that `@`-imports the three cache files. Cowork auto-loads `CLAUDE.local.md` every session and inlines `@`-imports with no tool call, so foundation + context become ambient grounding for free. The block also embeds the **workspace-pin working rule** (`workspace_id` from `link.json`) so any Stratafy MCP operation done in this folder targets the bound workspace — read *and* write are covered, not just grounding.

Read `CLAUDE.local.md`, then surgically replace only the `stratafy:begin … stratafy:end` block (create the file with a minimal header if absent; append the block if the file exists without sentinels). NEVER touch `CLAUDE.md` (that's the user's Folder-instructions surface) and NEVER rewrite the whole `CLAUDE.local.md` when sentinels exist — preserve everything outside the block. Timestamp-free; volatile state stays in `link.json`. Exact block shape and merge rules are in the **workspace-sync** skill.

If `$PROJECT_ROOT/.git` exists, ensure `.gitignore` contains `.stratafy/` and `CLAUDE.local.md` (append if missing). Skip silently if no `.git`.

### Step 5: Display — what changed, then freshness

If a previous `.stratafy/context.md` / `foundation.md` existed, diff against it and lead with what *changed* (turns sync from a chore into a briefing). Otherwise just summarise.

```
Synced {{workspace_name}} from Stratafy.

Changes since last sync:
{{e.g. "+ new key priority: Distributed Leadership Activation" / "~ mission reworded" / "no changes"}}

Foundation: {{mission one-liner}} · {{n}} values · {{n}} beliefs · {{n}} principles
Context:    {{n}} active strategies · {{n}} key priorities
Loadout:    {{team_name}} — {{n}} objectives · {{n}} metrics · {{n}} recent decisions

✓ Grounding block refreshed in CLAUDE.local.md (auto-loads next session)
Last synced: {{timestamp}}. Auto-refreshes after {{sync_ttl_days}} days.
Force refresh: /stratafy:sync (say "refresh")
```

### Honest empty state

If foundation or context is empty/sparse, say so — never fabricate:

> *"{{workspace_name}}'s foundation in Stratafy is still being populated — some sections are in draft."*

If `get_user_strategy` returns `scope: "role"` or `"empty"`, surface it plainly — never present a role-fallback or empty loadout as a team loadout:

> *"You're not linked to a team in {{workspace_name}} yet — the loadout falls back to the strategies you lead."* — or — *"No team objectives to load yet."*

## Provenance Context

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"sync"`
- `_change_reasoning`: e.g., `"Cache miss / {{n}}-day TTL expiry for workspace {{name}}"`

## Rules

1. NEVER fetch without a link — if unlinked, run the workspace-link flow inline first, then continue
2. NEVER call `get_workspace_snapshot` without `sections` — the full payload overflows context
3. NEVER fabricate foundation/context to fill gaps — surface sparse state honestly
4. NEVER write to `~/.stratafy/` — always the absolute project-rooted path
5. NEVER write to `CLAUDE.md` — the plugin owns only `CLAUDE.local.md`; `CLAUDE.md` is the user's
6. NEVER rewrite `CLAUDE.local.md` wholesale when sentinels exist — replace only between the markers
7. ALWAYS re-assert the managed block on every sync, timestamp-free, so a fresh `@`-import is in place
8. ALWAYS update `link.json` `last_synced` after a successful write, preserving other fields
9. Respect the TTL — don't re-fetch fresh cache unless the user explicitly forces a refresh
10. NEVER fabricate a team or its objectives — `get_user_strategy` is caller-scoped; surface `role` / `empty` scope honestly
