---
description: Show in-flight or completed Genesis run state, including Capture Score breakdown
---

# /stratafy-genesis:status

Read-only view of Genesis run state. Use to:

- Check progress of an in-flight bootstrap (paused at a Review Gate)
- Inspect the Capture Score breakdown of a completed run
- See which experts contributed which entities (provenance audit)
- Diagnose why Phase 2 Runtime hasn't activated

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "status"`, `plugin_name: "stratafy-genesis"`.

### Step 2: Identify Target Run

If multiple runs exist on the workspace, ask the founder which run. Default: most recent.

A Genesis run is identifiable by entities tagged `_source_plugin: "stratafy-genesis"` with a shared `_source_ref: "<run_id>"`.

### Step 3: Gather Run State

In parallel:

- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]` — what got written
- `get_pending_decisions` — unresolved cross-expert conflicts from this run
- `list_strategies` — scan provenance for `_source_expert` distribution

If a Capture Score MCP tool is available (sibling initiative `cb632f19` — Genesis Capture Score Engine), call:

- `compute_genesis_capture_score` with `run_id: <id>`

If not yet available, present the score as "(pending — Capture Score Engine not yet shipped)".

### Step 4: Present

```
━━━ GENESIS RUN STATUS ━━━━━━━━━━━━━━━

Run ID:  <id>
Started: <timestamp>
Mode:    [founder | coach]
Wall-clock: <minutes>m
State:   [in-flight @ Gate 1 | in-flight @ Gate 2 | completed | aborted]

━━━ CAPTURE SCORE ━━━━━━━━━━━━━━━━━━━━

Composite: <score>/100   [or: pending — engine not yet shipped]
  Founder-problem fidelity:   <score>/100
  Market framing coherence:   <score>/100
  Strategic coherence:        <score>/100
  Measurability of commitments: <score>/100
  Risk surface adequacy:      <score>/100

(Capture Score grades how well Genesis interpreted the seed.
 Strategy Health Score — workspace-level structural quality —
 is shown separately via /stratafy-guardian:scan.)

━━━ EXPERT CONTRIBUTIONS ━━━━━━━━━━━━━

CMO:        <N> entities authored
CTO:        <N> entities authored
FD:         <N> entities authored
GC:         <N> entities authored
CHRO:       <N> entities authored (if dispatched)
Reconciler: <N> cross-expert merges

━━━ PENDING DECISIONS (<N>) ━━━━━━━━━━

[For each pending decision tagged with this run_id:]
  • <description>
    Conflict: <expert A> says X / <expert B> says Y
    → Resolve via /stratafy-team:resolve-decision <id>

━━━ PHASE 2 RUNTIME ━━━━━━━━━━━━━━━━━━

[active | not yet activated | activation failed: <reason>]

[If active:]
  Autonomous expert contributions in last 7 days: <N>
  (Target for Done definition: ≥ 5 in first 7 days)
```

## Provenance Context

`status` is read-only — no mutations, no provenance tags needed beyond standard `get_user_context` logging.

## Rules

1. **Be explicit about pending engines.** If Capture Score Engine isn't shipped yet, say so — don't fabricate a score.
2. **Capture Score and Strategy Health Score are different.** Never present them as the same number, never average them, never substitute one for the other.
3. **Surface the Phase 2 success metric.** "Done when… ≥ 5 autonomous expert contributions land in the first 7 days post-bootstrap" is the closeout criterion for the Genesis Phase 1 Orchestrator initiative — show progress against it.
4. **Run-by-run, not aggregate.** This command is per-run state, not a portfolio view of all Genesis runs ever. Default to most recent; require explicit run ID for older runs.
