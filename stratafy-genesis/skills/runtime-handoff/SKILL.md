---
name: runtime-handoff
description: Activate Expert Agent Runtime on a freshly-bootstrapped workspace; Genesis ends, ambient continuation begins
---

# Runtime Handoff

The closeout step. Genesis Phase 1 ends with this skill. From here, Expert Agent Runtime (sibling strategy `2ce9f836`) takes over — experts wake on triggers (entity edits, metric changes, alignment warnings) on their own cadences.

**Genesis is not a Phase 2 primitive.** This is a critical boundary: Genesis does not own anything that runs after Phase 1 completes. The handoff is one MCP call.

## When to Run

Only after `write-through` has completed successfully. If write-through reported failures, the handoff is still appropriate — partial bootstraps still benefit from ambient continuation, which can fill gaps over time.

## Hard Rules

1. **No scheduling.** Do not propose, suggest, or implement a "Genesis day-6" or "Genesis day-7" follow-up. The previous Genesis Week framing was explicitly dropped.
2. **No calendar triggers.** No "weekly Genesis review", no "monthly Genesis re-run". The plugin does not own runtime concerns.
3. **One MCP call.** The handoff itself is a single `enable_expert_agent_runtime` call (or its current equivalent name in the MCP registry). If that tool isn't yet shipped, skip the handoff and surface a clear note to the founder.

## The Call

```typescript
// Target call (Expert Agent Runtime exposes this MCP tool when ready):
enable_expert_agent_runtime({
  workspace_id: <current workspace>,
  experts: ['cmo', 'cto', 'fd', 'gc', 'chro'],  // the 5 experts that contributed to bootstrap
  trigger_modes: ['entity_edit', 'metric_change', 'alignment_warning'],
  _source_plugin: 'stratafy-genesis',
  _source_command: 'bootstrap',
  _source_expert: 'reconciler',
  _change_reasoning: 'Genesis Phase 1 closeout — handing continuation to Expert Agent Runtime per sibling strategy 2ce9f836',
  _source_ref: '<run_id>',
})
```

## If the Tool Isn't Available Yet

Expert Agent Runtime is a sibling strategy — it may not be shipped yet at the time the founder runs Genesis. Detection logic:

```
1. Attempt the call.
2. If it returns "tool not found" or equivalent → fallback: surface a notice
   to the founder explaining that Phase 2 Runtime activation is pending the
   Expert Agent Runtime strategy shipping. Do NOT fabricate activation.
```

Notice template:

```
⚠️  Phase 2 Runtime not yet activated

Expert Agent Runtime (sibling strategy 2ce9f836) is not yet shipped.
Your bootstrapped workspace is fully written, but experts will not
auto-wake on triggers until Runtime ships.

For now: re-run Genesis commands manually as you evolve the workspace,
or use the individual /stratafy-expert-* plugins for targeted contributions.

The Genesis Phase 1 Orchestrator initiative tracks Runtime activation
as part of its Done definition: ≥ 5 autonomous expert contributions in
the first 7 days post-bootstrap.
```

## Handoff Confirmation

On successful activation, confirm the boundary explicitly:

```
✅ Phase 2 Runtime activated.

From here:
  - CMO will wake when market signals change or when a strategy's
    market framing is edited.
  - CTO will wake when a technical risk is added or when a build
    decision is logged.
  - FD will wake when a metric crosses a threshold or unit economics
    inputs change.
  - GC will wake when a regulatory signal arrives or when jurisdiction
    expands.
  - CHRO will wake when org positions change or hiring proposals are filed.

Genesis is done. Run /stratafy-genesis:engage on a different workspace
when you want to bootstrap another seed.
```

## Provenance

The handoff itself is a write — it changes workspace runtime state. Provenance:

- `_source_plugin: "stratafy-genesis"`
- `_source_command: "bootstrap"`
- `_source_expert: "reconciler"` (the handoff is the reconciler's last act, not any single expert)
- `_change_reasoning`: "Genesis Phase 1 closeout — handing continuation to Expert Agent Runtime per sibling strategy 2ce9f836"
- `_source_ref: "<run_id>"`

## Anti-Patterns

- **Suggesting "weekly Genesis check-ins".** Wrong primitive. Use `/stratafy-team:weekly-review` or `/stratafy-guardian:scan` for that — they're owned by other strategies.
- **Re-running Genesis on a workspace that already has a completed Genesis run.** That's a re-bootstrap, not continuation. Surface to the founder: "This workspace was bootstrapped on <date>. A fresh Genesis run will write new entities alongside existing ones. Continue?"
- **Activating Runtime with a subset of the standup experts** (e.g., only CMO + CTO). All experts that contributed to the bootstrap should activate, so the workspace's ambient coverage matches what produced it.
