---
description: Generate a pitch deck on demand from a completed Genesis run
---

# /stratafy-genesis:pitch

Generate a pitch deck from a completed Genesis run. Per the Genesis strategy, pitch generation is **on-demand, not scheduled** — there is no Genesis day-6 or day-7 trigger.

## When to Use

- After `bootstrap` completes and the founder wants to share the workspace as a deck
- Coach prepping for a client meeting and wants a board-ready summary of the pre-run
- Updating an existing pitch from a workspace that has evolved since the last generation

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "pitch"`, `plugin_name: "stratafy-genesis"`. Detect mode (founder / coach).

### Step 2: Confirm Source Run

Ask the founder which run / workspace state to pitch from. Default: most recent Genesis bootstrap on the active workspace.

If the workspace has been edited since the bootstrap (Phase 2 Runtime contributions, manual edits), confirm whether the pitch should reflect:

- **As-bootstrapped state** — what Genesis produced at Phase 1 completion (use snapshot)
- **Current live state** — what the workspace looks like now

Most use cases want current. As-bootstrapped is for "show me what Genesis interpreted from my seed" demos.

### Step 3: Gather Workspace Content

In parallel:

- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities", "metrics"]` (NEVER call without sections)
- `list_signals` filtered to `priority: high` or `priority: critical`
- `list_risks` filtered to high-severity (use `get_risks_for_context` for each top-level strategy rather than unfiltered list)
- `list_assumptions` similarly scoped

### Step 4: Build Slide Plan

Standard board-ready deck structure:

1. **Cover** — workspace name, tagline, "Composed by Stratafy Genesis on <date>"
2. **The problem** — from CMO P0 problem statement
3. **The wedge / beachhead** — from CMO positioning + beachhead
4. **The market signals** — top Radar signals
5. **The product / strategy** — top-level strategies as the architecture
6. **Unit economics** — from FD strategy + key metrics
7. **Why now** — from foundation beliefs + market signals
8. **The team / org plan** — from CHRO standup output if available, else "to be filled"
9. **Risks & how we mitigate** — top risks with mitigation status
10. **Capital ask** — from FD `capital_needs_estimate_usd` (if available) or "to be filled"
11. **Closing — what we're building** — from mission + vision

In coach mode, swap framing for advisor/consultant audience:

- Cover: "Strategic snapshot for <client name>"
- Closing: "Recommended next conversations" instead of "What we're building"

### Step 5: Generate the Deck

Use the existing presentations layer:

- `create_presentation` with `_source_plugin: "stratafy-genesis"`, `_source_command: "pitch"`
- For each slide in the plan, `add_slide_to_presentation` with the slide content

Reference the source entities by `urls.detail` so the founder can drill in from the deck.

### Step 6: Present the Output

```
━━━ PITCH GENERATED ━━━━━━━━━━━━━━━━━━

Presentation: <title>
Slides: <N>
Source state: [as-bootstrapped | current live state]

URL: <presentation.urls.detail>

→ Edit slides directly in the workspace
→ Export as PDF / Slides via the presentation actions
```

## Provenance Context

On every mutation, include:

- `_source_plugin: "stratafy-genesis"`
- `_source_command: "pitch"`
- `_source_expert: "cmo"` (CMO is the natural author of pitch narrative)
- `_change_reasoning: "On-demand pitch generation from Genesis-bootstrapped workspace state on <date>"`
- `coach_session: true` (only in coach mode)

## Rules

1. **No calendar trigger, ever.** Pitch is on-demand. Do not propose scheduling, "weekly pitch", or "auto-update on milestone".
2. **Sectioned `get_workspace_snapshot` always.** Unfiltered call overflows context. See the rules in `/stratafy-plugins`.
3. **Source state is the founder's call.** Don't auto-pick. The "as-bootstrapped" view is the Genesis demo asset; the "current" view is the working deck.
4. **In coach mode, the deck is for the coach to share with the client** — frame accordingly. Default to current state, default to advisor framing.
5. **Honest about gaps.** If the FD didn't produce a `capital_needs_estimate`, the slide says "to be filled" — do not invent a number.
