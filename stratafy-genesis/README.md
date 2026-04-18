# Stratafy Genesis

**Compose a complete Stratafy workspace from a seed idea — in a single Cowork session.**

Genesis dispatches an AI founding team (CMO, CTO, FD, GC, CHRO) in parallel against your seed, reconciles their output, and writes a coherent foundation + strategy tree through to Stratafy with per-expert provenance. Target wall-clock: under 15 minutes.

This is the apex demo of Stratafy-as-infrastructure. One session, seed to live workspace, visible breadth across every Stratafy layer.

## Who it's for

- **Founders** building a workspace from an idea — drop a paragraph, watch a draft strategy emerge live.
- **Coaches and consultants** prepping a client session — pre-run Genesis on a client URL or transcript and walk into the meeting with a draft to react to.
- **VCs** describing a portfolio-co thesis live and watching a workspace build.

Mode (founder vs coach) is auto-detected from your user context.

## Commands

| Command | What it does |
| --- | --- |
| `/stratafy-genesis:engage` | Start a session — auto-detect mode, list any in-flight runs, and route you to the next action |
| `/stratafy-genesis:bootstrap` | Run the full Phase 1 pipeline from a seed → live Stratafy workspace |
| `/stratafy-genesis:pitch` | Generate a pitch deck on demand from a completed Genesis run |
| `/stratafy-genesis:status` | Show in-flight or completed run state, including Capture Score |

## Skills

The plugin ships with seven internal skills that map 1:1 to the Genesis pipeline phases:

| Skill | Phase | Role |
| --- | --- | --- |
| `seed-intake` | Pre-P0 | Structure a raw founder paragraph into the 6 P0 input fields |
| `p0-market-radar` | P0 | CMO + Radar parallel pass — problem, beachhead, positioning, signals |
| `p1-veto-pass` | P1 | GC + CTO + FD parallel veto — regulatory landmines, buildability, unit economics |
| `p2-mission-vision` | P2 | CMO mission/vision draft → Founder-Persona revision |
| `p3-reconciliation` | P3 | Resolve cross-expert conflicts into a coherent L1 strategy proposal |
| `write-through` | Write | Provenance-stamped writes to Stratafy with per-expert attribution |
| `runtime-handoff` | Phase 2 | Activate Expert Agent Runtime for ambient continuation post-bootstrap |

## How it works

1. **Seed intake.** You drop a paragraph. Genesis structures it into product concept, beachhead hypothesis, jurisdiction, revenue model, vertical, and scale ambition. Missing fields surface as questions for you. **(Review Gate 1)**
2. **Parallel standup (P0 + P1).** CMO and Radar dispatch in parallel for market framing and signal scan. GC, CTO, and FD dispatch in parallel for a veto pass — anything that would kill the venture before it ships.
3. **Mission and vision (P2).** CMO drafts mission and vision; the Founder-Persona revises in your voice.
4. **Reconciliation (P3).** Conflicts between experts surface and resolve. Anything that can't auto-resolve becomes a `pending_decision` for you. **(Review Gate 2)**
5. **Write-through.** Reconciled output writes through to Stratafy — foundation, top-level strategies, signals, assumptions, risks — every entity tagged with which expert authored it.
6. **Phase 2 handoff.** Genesis ends with `enable_expert_agent_runtime` — your experts wake on triggers from there, not on a calendar.

## Self-contained V1

Genesis ships with slim internal expert personas. It does **not** require the full `stratafy-expert-*` plugins to be installed. V2 will detect installed expert plugins and delegate to them where present.

## Capture Score vs Strategy Health Score

Genesis introduces a **Capture Score** that grades how well Stratafy interpreted your seed and captured it as a coherent strategy — attached to a Genesis run. This is distinct from the existing **Strategy Health Score**, which grades structural workspace quality independent of provenance and is attached to a workspace. They sit alongside, not on top of each other.

## Reference seed

The reference seed for testing and demo is a **Legal ODR (Online Dispute Resolution) platform** — two-sided dispute resolution with AI coordination and a human lawyer panel. Chosen to exercise regulatory, multi-party, evidence-validation, and human-expert-panel dimensions simultaneously.

## Provenance

On every mutation tool call (`create_*`, `update_*`, `delete_*`, `link_*`, etc.), Genesis always includes:

- `_source_plugin`: `"stratafy-genesis"`
- `_source_command`: the command being run (e.g., `"bootstrap"`, `"pitch"`)
- `_change_reasoning`: 1-2 sentences explaining WHY this change is being made
- `_source_expert`: which slim persona authored the write (`cmo`, `cto`, `fd`, `gc`, `chro`, or `reconciler` for cross-expert merges)

In coach mode, an additional `coach_session: true` tag is added so the founder later knows which writes were authored on their behalf during a pre-session prep run.

## Status

**v0.2.0 — Phase 2 wired.** The plugin now closes the full loop: bootstrap writes a workspace AND activates Expert Agent Runtime so the autonomous scan jobs start firing on the platform's existing 5-minute cron.

- ✅ Bootstrap end-to-end (intake → P0+P1 standup → P2 → P3 reconciliation → write-through)
- ✅ Per-expert provenance on every entity
- ✅ `assign_expert_to_strategy` on every Phase 1 strategy (so the runtime knows who owns what)
- ✅ `enable_expert_agent_runtime` activation at closeout (idempotent, audit-tracked)
- ⏳ Server-side `genesis-orchestrator` MCP tool (replaces conversational dispatch — coming)

**v0.1.0** shipped the conversational orchestration; v0.2.0 closes the Phase 2 handoff that was the missing 2% of the Genesis Phase 1 Orchestrator initiative.
