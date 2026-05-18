---
name: "workspace-link"
description: "Links a Claude Desktop / Claude Code project folder to a Stratafy workspace. Discovers the workspaces the authenticated identity can access via select_workspace (no ID), lets the user pick, and writes <project-root>/.stratafy/link.json. Use when the user runs /stratafy:link, OR auto-trigger inline when any stratafy-core operation needs workspace context but <project-root>/.stratafy/link.json is absent — offer to link before proceeding rather than failing."
---

# Workspace Link

Owns the project↔workspace binding. The binding is a local file (`<project-root>/.stratafy/link.json`) inside the user's project folder, so it's visible to file tools in both Claude Code CLI and Claude Desktop / Cowork (Cowork only exposes the selected project folder).

This is the generic replacement for hardcoded workspace IDs. The workspace a project belongs to is **data in a file**, discovered and chosen once, not a constant baked into skills.

## Trigger

Use when:

- The user runs `/stratafy:link`
- **Auto-trigger inline:** any stratafy-core operation (`/stratafy:sync`, `/stratafy:status`, or any context-needing read) finds `<project-root>/.stratafy/link.json` absent. Do NOT fail with "run /stratafy:link first" — instead run this flow inline ("This project isn't linked to a Stratafy workspace yet — here are the ones you can access…"), then continue the original operation.
- The user asks to switch this project to a different workspace

## Resolving the project root

Same discipline as every stratafy-core local-file operation:

| Environment | Project root |
|---|---|
| Claude Code CLI | `$CLAUDE_PROJECT_DIR`, else `pwd` |
| Claude Desktop / Cowork | the selected project folder (absolute path); `pwd` is a scratchpad, not the project |

```bash
PROJECT_ROOT="${CLAUDE_PROJECT_DIR:-$(pwd)}"
mkdir -p "$PROJECT_ROOT/.stratafy"
echo "$PROJECT_ROOT/.stratafy/link.json"   # absolute path for Write
```

The Write tool does not expand `~` or resolve relative paths — always pass the absolute path.

## Discovery

Call `select_workspace` with **no `workspaceId`**. This returns the workspaces the authenticated identity can access. Required provenance: `_llm_model`, `_intent: "user_request"`, `_reason`, `_source_plugin: "stratafy-core"`, `_source_command: "link"`.

- 0 returned → surface honestly; never fabricate. *"No Stratafy workspaces available for your account — check you're signed into the right Stratafy identity."*
- 1 returned → confirm rather than ask
- 2+ returned → numbered list, name + role, user picks

## Validate + capture identity

After the user picks, call `get_user_context(workspace_id: <chosen>, …)`. This validates access, pins the session, and returns the user identity — all in one round-trip. Capture `user.email` for `linked_by`. Do NOT call `select_workspace` again for the chosen ID; `get_user_context` with `workspace_id` does the pin.

## Link file

`<project-root>/.stratafy/link.json`:

```json
{
  "version": "1.0",
  "workspace_id": "<uuid>",
  "workspace_name": "<name>",
  "linked_at": "<ISO8601>",
  "linked_by": "<user.email>",
  "last_synced": null,
  "sync_ttl_days": 7
}
```

`last_synced` is `null` until the workspace-sync skill pulls context. Re-linking to a different workspace overwrites the file and resets `last_synced` to `null`.

## After linking

Offer an immediate sync (hand off to the workspace-sync skill). If the user declines, tell them context will be pulled the first time it's needed (the auto-trigger covers it).

## Honest failure

If `select_workspace` fails (auth, network, missing MCP config), surface the error with a recovery path — do NOT pretend a link succeeded:

> *"Couldn't reach Stratafy to list workspaces. Error: {{message}}. Check the connection and try `/stratafy:link` again."*

## Provenance

- `_source_plugin`: `"stratafy-core"`
- `_source_command`: `"link"`
- `_change_reasoning`: e.g., `"Linking project to workspace {{name}}"`

## Local Files

- `<project-root>/.stratafy/link.json` — the project↔workspace binding (written here, read by workspace-sync and status-check)

## Rules

1. NEVER hardcode a workspace ID — discover via `select_workspace` (no ID), always
2. NEVER fail with "run /stratafy:link first" when invoked inline — run the link flow then continue the original operation
3. NEVER fabricate a workspace list — empty means empty, say so
4. NEVER write to `~/.stratafy/` — always the absolute project-rooted path
5. Re-linking resets `last_synced` to `null` — the new workspace's context is not yet local
