---
name: write-through
description: Write the reconciled L1 proposal to Stratafy with per-expert provenance on every entity
---

# Write-Through

The persistence step. Take the P3 reconciliation output and write it through to Stratafy. Every entity carries provenance tagged with the authoring expert.

This is the only skill that mutates workspace state. Everything before it is in-conversation analysis.

## Hard Pre-Conditions

Do NOT call this skill unless ALL are true:

1. The P3 `ReconcileP3Output` has passed all quality gates from the `p3-reconciliation` skill
2. The founder has explicitly approved at Review Gate 2 (in the `bootstrap` command flow)
3. The aggregated P1 verdict was NOT `stop`
4. `_source_plugin: "stratafy-genesis"` is set on the active session

If any pre-condition fails, abort and surface the reason. Never write partial Genesis output.

## Write Order

Order matters because of foreign-key relationships and the alignment scan downstream:

### Phase A — Foundation

1. `update_mission` with mission text — `_source_expert: "founder-persona"` (the revised version, not CMO draft)
2. `update_vision` with vision text — `_source_expert: "founder-persona"`
3. For each value: `create_value` — `_source_expert: <authored_by>`
4. For each principle: `create_principle` — `_source_expert: <authored_by>`
5. For each belief: `create_belief` — `_source_expert: <authored_by>`

### Phase B — Strategies

For each strategy in P3 output:

- `create_strategy` with name, description, strategy_type, horizon_phase, priority
- `_source_expert: <authored_by>` (or `"reconciler"` for cross-expert merges)

Capture the returned `id` for each strategy — needed for Phase D linking.

### Phase C — Signals, Risks, Assumptions

For each entity:

1. Create the entity (`create_signal`, `create_risk`, `create_assumption`) with `_source_expert`
2. Link to strategies via `link_entities`:
   - Signals: `source_type: "signal"`, `target_type: "strategy"`, `impact_level: <from entity>`
   - Risks: `source_type: "risk"`, `target_type: "strategy"`, `exposure_level: <from severity>`
   - Assumptions: `source_type: "assumption"`, `target_type: "strategy"` (use `link_assumption_to_context` if `link_entities` rejects)

### Phase D — Pending Decisions

For each pending decision:

- `create_decision` with status `pending`, full conflict summary in description
- `_source_expert: "reconciler"` (no single expert authored a conflict)
- Tag with `_source_ref: "<run_id>"` so they're traceable to this Genesis run

### Phase E — Alignment Suite

Run once on the completed bootstrap:

- `run_alignment_suite` — surfaces structural gaps as alignment scan results
- For any gap of severity `high` or `critical`, create a `pending_decision` referencing the gap

This is the post-write QA gate — alignment failures don't block the bootstrap (the workspace is already written), but they surface for founder action.

## Provenance on Every Call

Every mutation tool call must include:

```typescript
{
  _source_plugin: "stratafy-genesis",
  _source_command: "bootstrap",  // or "pitch" if write-through is invoked from /pitch
  _source_expert: <authored_by>, // 'cmo' | 'cto' | 'fd' | 'gc' | 'chro' | 'founder-persona' | 'reconciler'
  _change_reasoning: "Genesis Phase 1 bootstrap from seed: <one-line seed summary>",
  _source_ref: "<run_id>",       // ties all writes from this run together
  // Coach mode only:
  coach_session: true,
}
```

## Idempotency

A re-run of write-through on a partially-written workspace must NOT duplicate entities. Before each write:

1. Check if an entity with the same canonical name already exists in the workspace
2. If yes AND it carries `_source_plugin: "stratafy-genesis"` from the same `_source_ref`, skip it (already written)
3. If yes AND it carries different provenance (founder edited it manually since), skip it AND surface a warning: "Skipping write of value 'agency' — already exists with manual edits. Genesis will not overwrite founder edits."

## Error Handling

If any single entity write fails:

1. Log the failure with the entity name and reason
2. Continue with the remaining writes — partial bootstrap is better than full rollback
3. Surface all failures at the end as a "writes that didn't happen" list with retry instructions

Genesis writes are eventually-consistent in spirit; one CMO assumption failing to write should not abort the entire foundation persistence.

## Reference Implementation

The server-side equivalent is at `layers/genesis/server/services/genesis-write-adapter.ts` and `genesis-write-through.ts` in the main Stratafy repo. When those land as MCP-exposed tools, swap the per-entity calls for a single `genesis_apply_write_spec` MCP call.

## Anti-Patterns

- **Writing without `_source_expert`.** This destroys the per-expert provenance that is the whole point of Genesis. Reject the write.
- **Writing entities not in the P3 output.** If the founder casually mentioned "we should also track X" during Review Gate 2 chat, do NOT add X. Genesis writes only what passed reconciliation.
- **Bulk writes without per-entity error handling.** A 500 on one entity should not lose the other 19.
- **Writing in coach mode without the `coach_session: true` tag.** This is how the founder later distinguishes coach-prep writes from their own writes.
