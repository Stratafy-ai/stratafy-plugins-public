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

Once the seed is confirmed, dispatch the standup via the tool-restricted sub-agents shipped with this plugin. Each expert is a dedicated agent definition in `agents/` with `tools: Read` — **no MCP tools inherit into the sub-agents**, so prompt-size never blows up on the Stratafy tool surface.

**Run in parallel** (single message, 5 Agent tool uses, one per expert):

| Pass | Expert | `subagent_type` |
| --- | --- | --- |
| P0 | CMO | `stratafy-genesis:cmo-expert` |
| P0 | Radar | `stratafy-genesis:radar-expert` |
| P1 | GC | `stratafy-genesis:gc-expert` |
| P1 | CTO | `stratafy-genesis:cto-expert` |
| P1 | FD | `stratafy-genesis:fd-expert` |

Each Agent tool call passes the structured seed JSON in the prompt. For FD, also pass CMO's beachhead_options in the prompt so FD can produce unit-economics-by-wedge that matches CMO's naming — otherwise reconciliation downstream cannot merge.

Do **NOT** staple conversation history, canonical prompt file contents, or workspace snapshots into the sub-agent prompt — the agent body already carries the persona and schema, and the parent conversation bloats input tokens without adding signal. Seed JSON + lane-specific prior outputs only.

The skills `p0-market-radar` and `p1-veto-pass` remain as conceptual reference for the orchestrator — they are NOT passed to the sub-agents.

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

- The orchestrator deterministically synthesises a CMO mission/vision stub from P0 output (per `synthesiseCmoP2Draft` logic — inline in the main agent, no sub-agent needed).
- **Dispatch Founder-Persona via the plugin's tool-restricted sub-agent** — `subagent_type: "stratafy-genesis:founder-persona"`. Pass the seed, founder context (from `get_user_context`), and the CMO stub draft in the Agent prompt. No conversation history stapled in.

Stream both the CMO stub and the Founder-Persona revision back so the founder sees the revision happen.

### Step 6: P3 Reconciliation

Use the `p3-reconciliation` skill.

**Dispatch Reconciliation via the plugin's tool-restricted sub-agent** — `subagent_type: "stratafy-genesis:reconciler"`. Pass the full ReconcileP3Input (seed + all five P0/P1 expert outputs + CMO P2 draft + Founder-Persona P2 revision) in the Agent prompt.

The reconciler produces a compressed L1 strategy proposal: a coherent foundation + 3-5 top-level strategies + key signals/risks/assumptions, with conflicts flagged.

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

#### Step 8a — Workspace selection gate (MANDATORY, non-negotiable)

**Before ANY write-through call, explicitly echo the target workspace back to the founder and require affirmative confirmation.** This is a hard correctness gate, not a soft check. A Genesis run that writes to the wrong workspace is silent, destructive, and expensive to unwind — foundation + strategies + runtime activation all land in the wrong tenant.

Present this exact prompt:

```
━━━ WRITE-THROUGH TARGET ━━━━━━━━━━━━━━

  Workspace: <workspace_name>
  ID:        <workspace_id>

  I am about to write the foundation + strategy tree to this workspace.
  Type CONFIRM to proceed, or type the correct workspace name to switch.
```

Rules:
- Read the current workspace name + id via `get_user_context` or `select_workspace` with no arguments (list mode) before echoing.
- Accept ONLY `CONFIRM` (case-insensitive) to proceed.
- Any other response — including silence, "yes", "go", or a workspace name — is treated as a correction and triggers re-selection via `select_workspace`.
- After any `select_workspace` call mid-bootstrap, loop back to this gate. Never skip.
- If the current workspace is the Stratafy main workspace (the plugin author's own), treat that as a likely-wrong default and require an extra-explicit confirmation.

This gate runs EVERY time, not just when uncertain. It's cheap; the alternative is a customer's workspace getting polluted.

#### Step 8b — Write order + strategy_type discipline

Order: workspace metadata → foundation (mission, vision, values, principles, beliefs) → strategies (L0 root first, then L1 thrusts with parent_strategy_name) → signals/risks/assumptions linked to strategies → pending_decisions.

**`strategy_type` enum guard**: valid values are `company` (root only), `product`, `go-to-market`, `financial`, `technology`, `regulatory`, `people`, `team`, `operational`, `corporate`. `foundation` is NOT a valid strategy_type — it's reserved for mission/vision/values semantics. If the reconciler emits `strategy_type: foundation` for any thrust, override to `team` or `operational` based on the thrust's owning expert. Never ship `foundation` as a thrust type.

**Character encoding guard**: strategy names, descriptions, and all other string fields must contain LITERAL characters. If the reconciler output contains HTML entities (`&amp;`, `&lt;`, `&gt;`, `&quot;`, `&#39;`), decode them before passing to `create_strategy` / `create_risk` / other mutation calls. The reconciler's output schema is plain UTF-8, not HTML — entity-encoded names ship dirty and require manual cleanup.

#### Step 8c — Alignment suite with feedback loop

Run the alignment suite once on the completed bootstrap: call `run_alignment_suite`. **Poll for completion** (the runId is not actionable until the run finishes). Once complete, iterate over high/critical gaps:

- For each gap with severity `high` or `critical`, create a `pending_decision` with `_source_ref: alignment:<runId>:<gap_id>` and decision_type `type_2_reversible` unless the gap itself names a one-way door.
- Surface the count of auto-converted gaps in the closeout summary.

Alignment output that sits orphaned is not a QA pass. Either it converts to structured workspace entities, or the suite run was decoration.

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
2. **Dispatch P0 and P1 via the plugin's tool-restricted sub-agents** (`stratafy-genesis:<expert>`) in a single message with 5 Agent tool uses. These agents declare `tools: Read` — they cannot invoke MCP tools, cannot write to the workspace, and return JSON as their final message. Do NOT spawn generic-purpose sub-agents (they inherit the full 200+ Stratafy MCP tool surface and exceed input-token limits). If the Agent dispatch still fails on prompt size, fall back to inline dispatch from the main orchestrator rather than retrying the same bloated call.
3. **Stream each expert with persona attribution.** The founder watching live is the demo. A silent-then-summary pattern destroys the value.
4. **Honest provenance.** If the reconciler had to make a call between two experts, tag the entity `_source_expert: "reconciler"` — not the originating expert. Lying about authorship corrupts the change history.
5. **At the 12-minute mark, surface time pressure.** "We're at 12 minutes — P3 reconciliation is the longest remaining step. Continue or pause?" Long-running silently is failure.
6. **Idempotency on re-run.** Each expert reads current workspace state before writing. A re-run of `bootstrap` on a partially-seeded workspace must not duplicate entities — it refines.
7. **No expert overwrites another expert's work without a flagged refinement reason.** Write a `change_reasoning` like "CMO refined CTO's earlier wedge analysis after market signal X".
