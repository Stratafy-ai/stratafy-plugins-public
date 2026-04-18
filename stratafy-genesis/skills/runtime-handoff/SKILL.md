---
name: runtime-handoff
description: Activate Expert Agent Runtime on a freshly-bootstrapped workspace via enable_expert_agent_runtime; Genesis ends, ambient continuation begins
---

# Runtime Handoff

The closeout step. Genesis Phase 1 ends with this skill. From here, Expert Agent Runtime takes over — experts wake on cadence (default weekly), scan their linked strategies + metrics + assumptions + risks, and surface proposals + questions into the workspace.

**Genesis is not a Phase 2 primitive.** This is a critical boundary: Genesis does not own anything that runs after Phase 1 completes. The handoff is one MCP call — `enable_expert_agent_runtime` — which seeds default scan jobs from the per-role template library and an audit row in `workspace_runtime_activations`.

## When to Run

Only after `write-through` has completed successfully. If write-through reported failures, the handoff is still appropriate — partial bootstraps still benefit from ambient continuation, which can fill gaps over time.

## Hard Rules

1. **No scheduling.** Do not propose, suggest, or implement a "Genesis day-6" or "Genesis day-7" follow-up. The previous Genesis Week framing was explicitly dropped.
2. **No calendar triggers in the plugin.** The cron lives in the platform (`server/api/_cron/expert-scan-jobs.get.ts`), runs every 5 minutes, and dispatches workflows for any due scan job. The plugin's job is just to register the workspace; the platform does the rest.
3. **One MCP call.** The handoff itself is a single `enable_expert_agent_runtime` call. Idempotent — re-calling on an already-activated workspace returns the existing activation, no duplicate scan jobs.

## The Call

```typescript
enable_expert_agent_runtime({
  experts: ['cmo', 'cto', 'fd', 'gc', 'chro'],  // the 5 experts that contributed to bootstrap
  genesis_run_id: '<run_id>',                   // for end-to-end traceability
  _source_plugin: 'stratafy-genesis',
  _source_command: 'bootstrap',
  _source_expert: 'reconciler',
  _change_reasoning: 'Genesis Phase 1 closeout — activating Expert Agent Runtime so the just-written strategies start receiving autonomous expert contributions',
  _source_ref: '<run_id>',
})
```

The `workspace_id` is implicit — it's the active workspace from `select_workspace` / `get_user_context`.

## Response Shape

The tool returns an `ActivationRecord`:

```typescript
{
  id: string                          // activation row UUID
  workspace_id: string
  activated_at: string                // ISO timestamp
  experts_activated: string[]         // roles where scan jobs were actually seeded
  scan_jobs_created: string[]         // expert_scan_jobs row IDs
  was_created_now: boolean            // false if idempotent re-call
  expert_outcomes: Array<{
    role: string
    expert_id: string | null
    scan_jobs_seeded: number
    scan_job_ids: string[]
    skipped_reason: string | null     // 'expert_not_found_in_workspace' | 'no_v1_template_for_role' | null
  }>
  // ... source provenance fields
}
```

Use `expert_outcomes` to give the founder a per-expert breakdown. If any expert was skipped, name it explicitly — don't quietly drop it.

## Handoff Confirmation

On successful activation, confirm the boundary explicitly with the actual seeded jobs:

```
✅ Expert Agent Runtime activated.

Workspace registered: <workspace_id>
Experts activated:    <experts_activated.length>
Scan jobs seeded:     <scan_jobs_created.length>

[Per expert outcome:]
  ✓ CMO   — Weekly market & positioning scan         (next run: ~Mon)
  ✓ CTO   — Weekly architecture & tech-risk scan     (next run: ~Mon)
  ✓ FD    — Weekly unit economics & runway scan      (next run: ~Mon)
  ✓ GC    — Weekly regulatory & compliance scan      (next run: ~Mon)
  ✓ CHRO  — Bi-weekly org & people scan              (next run: ~Mon)

The platform cron polls every 5 minutes for due scan jobs and
dispatches the expert workflow per job. Proposals and questions
will land in the workspace — review them via:
  • /stratafy-team:proposals
  • /stratafy-guardian:scan
  • Or the workspace UI at /ws/<id>/agent

Genesis is done.

→ Run /stratafy-genesis:engage on a different workspace when you
  want to bootstrap another seed.
```

## When the Activation Returns `was_created_now: false`

This means the workspace was already activated. Don't re-seed jobs — surface to the founder:

```
ℹ️  Workspace already activated on <activated_at>.

Existing scan jobs are still active. No changes made.

If you want to add or modify scan jobs for an expert, use the
workspace agent UI directly — Genesis is not the right tool for
runtime config changes after bootstrap.
```

## When an Expert Was Skipped

If `expert_outcomes` includes a skip:

- `skipped_reason: 'expert_not_found_in_workspace'` → the expert was named but doesn't exist in the workspace's expert roster. This shouldn't happen at Genesis closeout (bootstrap should have seeded the roster), so flag it as an issue:
  > "⚠️ CHRO was requested but isn't in the workspace expert roster. The bootstrap may have skipped the CHRO seeding step. Re-run `seed_expert_roster` if needed."

- `skipped_reason: 'no_v1_template_for_role'` → expected for any expert outside cmo/cto/fd/gc/chro in V1. Note it but don't alarm:
  > "ℹ️ Advisor expert was requested but has no V1 scan template. Will be skipped until templates ship."

## Provenance

Standard for this skill:

- `_source_plugin: "stratafy-genesis"`
- `_source_command: "bootstrap"`
- `_source_expert: "reconciler"` (the handoff is the reconciler's last act, not any single expert)
- `_change_reasoning`: "Genesis Phase 1 closeout — activating Expert Agent Runtime so the just-written strategies start receiving autonomous expert contributions"
- `_source_ref: "<run_id>"`

## Anti-Patterns

- **Suggesting "weekly Genesis check-ins".** Wrong primitive. Use `/stratafy-team:weekly-review` or `/stratafy-guardian:scan` for that — they're owned by other strategies.
- **Re-running Genesis on a workspace that already has a completed Genesis run.** That's a re-bootstrap, not continuation. Surface to the founder: "This workspace was bootstrapped on <date>. A fresh Genesis run will write new entities alongside existing ones. Continue?"
- **Activating Runtime with a subset of the standup experts** (e.g., only CMO + CTO). All experts that contributed to the bootstrap should activate, so the workspace's ambient coverage matches what produced it.
- **Surfacing a "pending" notice when the tool is available.** As of plugin v0.2.0, `enable_expert_agent_runtime` ships in the platform MCP. The fallback notice should only fire on a verified tool-not-found error, not by default.
