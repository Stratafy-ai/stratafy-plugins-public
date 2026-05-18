---
description: "Link this project folder to a Stratafy workspace — pick from the workspaces you can access, then pull its foundation and key context"
---

# /stratafy:link

Link the current Claude Desktop / Claude Code project to a Stratafy workspace. Once linked, the project carries that workspace's foundation and key context locally, and keeps it in sync.

Run once per project. Re-runnable to switch the project to a different workspace.

## Process

### Step 1: Resolve project root + check existing link

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
LINK="$PROJECT_ROOT/.stratafy/link.json"
test -f "$LINK" && cat "$LINK" || echo "UNLINKED"
```

- `UNLINKED` → proceed to Step 2 (fresh link)
- Link present → show the current link (`workspace_name`, `last_synced`) and ask: *"This project is already linked to **{{workspace_name}}**. Re-link to a different workspace?"*
  - No → stop; nothing to do
  - Yes → proceed to Step 2, overwriting the link at the end

### Step 2: Discover available workspaces

Call `select_workspace` with NO `workspaceId` — this returns the workspaces the authenticated identity can access:

- `_llm_model`: `<your model>`
- `_intent`: `"user_request"`
- `_reason`: `"Listing available Stratafy workspaces for project linking"`
- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"link"`

### Step 3: Let the user pick

Present the returned workspaces as a short numbered list — name + role if available:

> "You have access to these Stratafy workspaces:
>
> 1. **{{name}}** — {{role}}
> 2. **{{name}}** — {{role}}
> 3. …
>
> Which one should this project be linked to?"

If only one workspace is returned, confirm rather than ask: *"Linking this project to **{{name}}** — your only workspace. Good?"*

If zero are returned, surface honestly: *"No Stratafy workspaces are available for your account. Check you're signed in to the right Stratafy identity, or contact your workspace admin."* — do not fabricate.

### Step 4: Validate + capture identity

Once the user picks, call `get_user_context` with the chosen `workspace_id` (this validates access AND pins the session AND returns the user identity in one round-trip):

- `workspace_id`: `<chosen uuid>`
- `command_name`: `"link"`
- `plugin_name`: `"stratafy-core"`
- `_llm_model`, `_intent: "user_request"`, `_reason`, `_source_plugin: "stratafy-core"`, `_source_command: "link"`

Capture `user.email` for the `linked_by` field.

### Step 5: Write the link file

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
mkdir -p "$PROJECT_ROOT/.stratafy"
echo "$PROJECT_ROOT/.stratafy/link.json"   # absolute path for the Write tool
```

Write to the absolute path. Shape:

```json
{
  "version": "1.0",
  "workspace_id": "{{chosen uuid}}",
  "workspace_name": "{{workspace name}}",
  "linked_at": "{{ISO8601 now}}",
  "linked_by": "{{user.email}}",
  "last_synced": null,
  "sync_ttl_days": 7
}
```

NEVER pass `~/.stratafy/...` to Write — the Write tool does not expand `~` or resolve relative paths. Always the absolute project-rooted path.

### Step 6: Sync now (default yes — don't make the user ask)

Linking without context is a half-done state — the user almost always wants the foundation immediately. So **proceed into sync by default**, with an explicit opt-out, rather than asking permission:

> "Linked to **{{workspace_name}}**. Pulling its foundation and key context now — say 'skip' if you'd rather not."

- No objection / "go" / silence → run the **workspace-sync** flow (one fetch; writes `.stratafy/foundation.md` + `context.md`, the managed grounding block in `CLAUDE.local.md`, and the `.gitignore` entries if it's a repo)
- "skip" → *"Linked. Run `/stratafy:sync` when you want the context — it'll also auto-pull the first time grounding is needed."*

After sync completes, the grounding block in `CLAUDE.local.md` `@`-imports the cache, so every future session in this folder is auto-grounded with no command and no tool call.

## Voice

Practical, calm, transparent. This is an infrastructure command — no salesmanship. The user is connecting a folder to a workspace; tell them exactly what that does.

## Provenance Context

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"link"`
- `_change_reasoning`: e.g., `"Linking project to workspace {{name}} per user request"`

## Rules

1. NEVER hardcode a workspace ID — always discover via `select_workspace` (no ID) and let the user pick
2. NEVER fabricate a workspace list — if the MCP returns none, say so honestly
3. NEVER write to `~/.stratafy/` — always the absolute project-rooted path
4. NEVER write to `CLAUDE.md` — the plugin owns only `CLAUDE.local.md`
5. Sync is the default after linking, not an offer — make the user opt OUT, not in
6. Re-linking overwrites `link.json` and resets `last_synced` to null (the new workspace's context hasn't been pulled yet)
7. The link is local to the project folder — switching projects means a different (or no) link
