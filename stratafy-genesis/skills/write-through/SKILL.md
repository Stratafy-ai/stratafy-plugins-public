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

### Phase B — Strategies (root + thrusts hierarchy)

P3 output gives you `strategies.root` (one) and `strategies.thrusts` (3-5). Write them in order:

**B.1 — Write the root strategy first**

```typescript
const root = await create_strategy({
  name: p3.strategies.root.name,
  description: p3.strategies.root.description,
  strategy_type: 'company',
  horizon_phase: 'now',
  priority: 'critical',
  // No parent_strategy_id — this IS the root.
  _source_expert: 'reconciler',
})
// Capture root.id — needed for Phase B.2.
```

The root does NOT get an expert assignment via `assign_expert_to_strategy` — it's owned by the founder. (V1.1 may add a `founder` expert role for this; until then, leave unassigned.)

**B.2 — Write each thrust as a child of root**

For each thrust in `p3.strategies.thrusts`:

1. `create_strategy` with:
   - `name`, `description`, `strategy_type`, `horizon_phase`, `priority` from the thrust
   - **`parent_strategy_id: root.id`** ← critical — this builds the hierarchy
   - `_source_expert: <authored_by>` (or `"reconciler"` for cross-expert merges)
2. **Immediately after**, call `assign_expert_to_strategy` with:
   - `expert_id`: the role-appropriate expert (looked up from the workspace expert roster — use `get_expert(role: <role>)` if not already cached)
   - `strategy_id`: the just-created thrust's `id`

Mapping of thrust strategy_type → owning expert:

| Strategy type | Owning expert role |
| --- | --- |
| `product` | cto |
| `go-to-market` | cmo |
| `finance` | fd |
| `regulatory` | gc |
| `people` / `org` | chro |
| `foundation` | reconciler (no single expert — leave unassigned, or assign to the expert that authored it most heavily) |

Why this hierarchy matters:

1. **Foundation (mission/vision) points at the root** — one coherent thing the company is building. Without a root, mission/vision orphan against three disconnected strategies.
2. **Thrusts get clean ownership** — each thrust has one expert who scans it, one health score, one set of objectives. Mixing the company-level view into a thrust pollutes that ownership.
3. **The Expert Agent Runtime works correctly** — each expert's scan job runs against their thrust + the root for context. Without a root, scans run against an undifferentiated workspace.
4. **Health checker stops flagging `missing_parent`** — the structural-health amber on every strategy disappears.

Capture all returned `id`s — root + each thrust — for Phase D linking AND for the per-thrust expert assignments above.

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
