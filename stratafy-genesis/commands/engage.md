---
description: Start a Genesis session — auto-detect mode, list any in-flight runs, and route to the next action
---

# /stratafy-genesis:engage

Entry point for a Genesis session. Detects mode (founder / coach), surfaces any in-flight or recently-completed runs, and routes to the right next command — usually `bootstrap`, sometimes `pitch` or `status`.

## Process

### Step 1: Get User Context

Call `get_user_context` with:

- `command_name: "engage"`
- `plugin_name: "stratafy-genesis"`

Inspect the returned `user_lens` and `role_mandate`. Infer mode:

- **Coach mode** if `user_lens` includes consultant/coach framing OR the user explicitly says "for [client name]" / "ahead of meeting with…"
- **Founder mode** otherwise (default)

### Step 2: Check for In-Flight Genesis Runs

Genesis runs are recorded as workspace activity with `_source_plugin: "stratafy-genesis"`. To find recent runs, call:

- `list_strategies` — scan results for any with `_source_plugin: "stratafy-genesis"` in their provenance metadata; these are workspaces seeded by a prior Genesis run
- `get_pending_decisions` — Genesis surfaces unresolved cross-expert conflicts here, so any pending decisions tagged `genesis_run_id` indicate a run that stopped at Review Gate 2

If there are no in-flight runs and the workspace is unseeded, this is a fresh Genesis. Skip to Step 3.

If there is an in-flight run with pending decisions, surface it — the user likely wants to resolve and continue, not start fresh.

If a recent completed run exists, offer `pitch` or `status` as alternatives to a new bootstrap.

### Step 3: Present Routing

```
GENESIS ENGAGE — [Date]

Mode: [founder | coach]
[If coach mode: Working on behalf of: <client name from context>]

━━━ WORKSPACE STATE ━━━
[Fresh / seeded by prior Genesis run on <date> / in-flight run paused at Review Gate 2]

[If in-flight:]
  Run ID: <id>
  Stopped at: Review Gate 2 (post-reconciliation, awaiting founder approval)
  Pending decisions: <N>
  Capture Score (preliminary): <score>/100

[If recently completed:]
  Last run: <date>, Capture Score <score>/100
  Active strategies: <N>
  Phase 2 Runtime: [active / not yet activated]

━━━ WHAT NEXT ━━━

[Branch on state:]

→ Fresh workspace: Run /stratafy-genesis:bootstrap with your seed
→ In-flight at Gate 2: Run /stratafy-genesis:bootstrap to resume
→ Completed run: Run /stratafy-genesis:pitch to generate a deck
                 Or /stratafy-genesis:status for full Capture Score breakdown
```

The plugin only exposes FOUR commands: `engage`, `bootstrap`, `pitch`, `status`. The seven skills (`seed-intake`, `p0-market-radar`, `p1-veto-pass`, `p2-mission-vision`, `p3-reconciliation`, `write-through`, `runtime-handoff`) are internal to commands — they have no slash invocation. **Never offer a skill name as a slash command.**

### Step 4: Wait for Direction

Do not auto-run any phase. The founder picks the next command. This is an entry point, not a launcher.

If the user pastes a seed paragraph after `engage` (without invoking `bootstrap`), do NOT silently start the pipeline. Respond:

> "Got the seed. Hit `/stratafy-genesis:bootstrap` and I'll run the full Phase 1 pipeline against it (intake → standup → reconciliation → write-through, with two founder gates along the way)."

The bootstrap command is the only entry point that runs work.

## Provenance Context

`engage` is a read-only command — no mutations, no provenance tags needed beyond the standard `get_user_context` logging.

## Rules

1. **Never auto-bootstrap.** A fresh seed is a deliberate act; require the user to invoke `bootstrap` explicitly.
2. **Surface paused runs first.** Resuming an in-flight run is almost always more valuable than starting fresh — the founder has already invested time in the seed.
3. **In coach mode, always identify the client by name** in the routing presentation, so the coach can confirm they're in the right workspace before any writes happen.
4. **Keep this scannable in 30 seconds.** This is routing, not analysis.
