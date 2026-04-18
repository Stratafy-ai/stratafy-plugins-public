---
description: Run the full Phase 1 Genesis pipeline — seed → standup → reconciliation → live workspace
---

# /stratafy-genesis:bootstrap

The main event. Take a seed paragraph, run the parallel multi-expert standup (CMO, CTO, FD, GC, CHRO), reconcile their output, and write a coherent foundation + strategy tree through to Stratafy with per-expert provenance.

Wall-clock target: under 15 minutes.

## Process

### Step 1: Get User Context & Seed

Call `get_user_context` with `command_name: "bootstrap"`, `plugin_name: "stratafy-genesis"`. Note `mode` (founder / coach) from the prior `engage` run or re-detect.

Ask for the seed paragraph if not already in conversation:

> "Drop the seed paragraph for the workspace you want to build. A few sentences is enough — what the product is, who it's for, where it launches, how it makes money."

### Step 2: Seed Intake (Pre-P0)

Use the `seed-intake` skill. Structure the raw paragraph into the 6 P0 input fields:

- `product_concept` (required)
- `beachhead_hypothesis`
- `launch_jurisdiction`
- `revenue_model_hypothesis`
- `industry_vertical`
- `scale_ambition`

For each field, attach a signal: `extracted` (clearly stated), `inferred` (LLM filled the gap), or `missing` (could not infer).

### Step 3: Review Gate 1 (Mandatory)

Present the structured seed back to the user:

```
━━━ STRUCTURED SEED ━━━━━━━━━━━━━━━━━━

Product concept: <text>           [extracted]
Beachhead:       <text>           [inferred ⚠️]
Jurisdiction:    <text>           [extracted]
Revenue model:   <text>           [inferred ⚠️]
Vertical:        <text>           [extracted]
Scale ambition:  <text>           [inferred ⚠️]

Questions for you (3 inferred fields):
  1. Beachhead — I inferred X. Does that match your intent, or are you targeting Y?
  2. Revenue model — I inferred Z. Confirm or correct?
  3. Scale ambition — I inferred vc-scale. If this is bootstrapped, say so now.

→ Confirm or correct each before I dispatch the standup.
```

**STOP HERE.** Wait for the founder to confirm or correct. Do NOT proceed to Step 4 until they have responded. Generic-output regressions originate at this gate.

### Step 4: P0 + P1 Parallel Standup

Once the seed is confirmed, dispatch the standup. Use the `p0-market-radar` and `p1-veto-pass` skills.

**Run in parallel** (single message, multiple Agent tool uses, one per expert):

| Pass | Expert | Agent prompt source |
| --- | --- | --- |
| P0 | CMO | `p0-market-radar` skill (CMO half) — problem statement, beachhead, positioning draft |
| P0 | Radar | `p0-market-radar` skill (Radar half) — environmental signals, market dynamics |
| P1 | GC | `p1-veto-pass` skill (GC) — regulatory landmines, jurisdiction risk, veto verdict |
| P1 | CTO | `p1-veto-pass` skill (CTO) — buildable today?, hard technical bits, budget advice, veto verdict |
| P1 | FD | `p1-veto-pass` skill (FD) — unit economics by wedge, capital needs, veto verdict |

Stream each expert's contribution back to the conversation **with persona attribution**:

```
🎯 CMO weighing in…
   <CMO P0 output>

🛡️  GC weighing in…
   <GC P1 verdict>

…
```

Aggregate the three P1 verdicts using the `aggregateP1Verdicts` rule (any `stop` → stop; else any `proceed_with_refinement` → proceed_with_refinement; else any `proceed_with_caveats` → proceed_with_caveats; else proceed).

If aggregated verdict is `stop`, halt the run and present the GC/CTO/FD reasoning. The founder decides whether to pivot the seed and re-run, or override.

### Step 5: P2 Mission & Vision

Use the `p2-mission-vision` skill.

- CMO drafts a stub mission/vision from `product_concept` and `positioning_draft` (per `synthesiseCmoP2Draft` logic)
- Founder-Persona revises in the founder's voice (use the `founder-p2.md` prompt as reference)

Stream both versions back so the founder sees the revision happen.

### Step 6: P3 Reconciliation

Use the `p3-reconciliation` skill.

Compose the standup input from P0 + P1 + P2 outputs. The reconciliation prompt produces a compressed L1 strategy proposal: a coherent foundation + 3-5 top-level strategies + key signals/risks/assumptions, with conflicts flagged.

Conflicts that auto-resolve are merged silently. Conflicts that cannot resolve become `pending_decisions` for the founder.

### Step 7: Review Gate 2 (Mandatory)

Present the reconciled L1 proposal back to the founder:

```
━━━ RECONCILED L1 PROPOSAL ━━━━━━━━━━━

Mission: <text>                        [authored by: founder-persona, revised from cmo]
Vision:  <text>                        [authored by: founder-persona, revised from cmo]

Top-level strategies (3):
  1. <strategy name>                   [authored by: cmo + reconciler]
  2. <strategy name>                   [authored by: cto + reconciler]
  3. <strategy name>                   [authored by: fd + reconciler]

Critical signals (5):    [authored by: radar]
Critical risks (3):      [authored by: gc + cto]
Key assumptions (4):     [authored by: cmo + fd]

⚠️  Pending decisions (2 unresolved conflicts):
  1. <conflict description> — <expert A position> vs <expert B position>
  2. <conflict description> — <expert A position> vs <expert B position>

→ Approve to write through to Stratafy, or revise.
[If coach mode: → Or hold for the meeting and share live with the client.]
```

**STOP HERE.** Wait for explicit founder approval before writing. Pending decisions can be left unresolved (they will land as `pending_decisions` entities in the workspace).

### Step 8: Write-Through

Use the `write-through` skill.

Once approved, write the foundation + strategy tree through to Stratafy. Every entity carries:

- `_source_plugin: "stratafy-genesis"`
- `_source_command: "bootstrap"`
- `_source_expert: "<authoring expert>"`
- `_change_reasoning: "Genesis Phase 1 bootstrap from seed: <one-line seed summary>"`
- `coach_session: true` (only in coach mode)

Order matters — foundation first (mission, vision, values, principles, beliefs), then strategies, then signals/risks/assumptions linked to the strategies, then any `pending_decisions`.

Run the alignment suite once on the completed bootstrap: call `run_alignment_suite`. Surface gaps as additional `pending_decisions`, not as blockers.

### Step 9: Phase 2 Handoff

Use the `runtime-handoff` skill.

Activate Expert Agent Runtime on the workspace. From here, experts wake on triggers — entity edits, metric changes, alignment warnings — on their own cadences. Genesis is done.

Present the closeout:

```
━━━ GENESIS PHASE 1 COMPLETE ━━━━━━━━━

Wall-clock: <minutes>m
Capture Score: <score>/100   (run-level — interpretation quality)
Strategy Health Score: <score>/100   (workspace-level — structural)

Written to workspace:
  - Foundation (mission, vision, 5 values, 4 principles, 3 beliefs)
  - 3 top-level strategies
  - 5 signals, 3 risks, 4 assumptions
  - 2 pending decisions awaiting your resolution

Phase 2 Runtime: ✅ activated
  Experts will now contribute on triggers — no scheduled wake-ups.

→ Run /stratafy-genesis:pitch when you want a deck
→ Run /stratafy-guardian:scan for a structural QA pass
```

## Provenance Context

On every mutation, include:

- `_source_plugin: "stratafy-genesis"`
- `_source_command: "bootstrap"`
- `_source_expert`: which slim persona authored the write
- `_change_reasoning: "Genesis Phase 1 bootstrap from seed: <seed summary>"`
- `coach_session: true` (only in coach mode)

## Rules

1. **Review Gate 1 and Review Gate 2 are not optional.** Skipping them is the single highest-risk failure mode (Risk 1 on the Genesis strategy — Generic Output Trap).
2. **Dispatch P0 and P1 in true parallel.** Single message, multiple Agent tool uses. Sequential dispatch breaks the wall-clock target and undermines the standup pattern.
3. **Stream each expert with persona attribution.** The founder watching live is the demo. A silent-then-summary pattern destroys the value.
4. **Honest provenance.** If the reconciler had to make a call between two experts, tag the entity `_source_expert: "reconciler"` — not the originating expert. Lying about authorship corrupts the change history.
5. **At the 12-minute mark, surface time pressure.** "We're at 12 minutes — P3 reconciliation is the longest remaining step. Continue or pause?" Long-running silently is failure.
6. **Idempotency on re-run.** Each expert reads current workspace state before writing. A re-run of `bootstrap` on a partially-seeded workspace must not duplicate entities — it refines.
7. **No expert overwrites another expert's work without a flagged refinement reason.** Write a `change_reasoning` like "CMO refined CTO's earlier wedge analysis after market signal X".
