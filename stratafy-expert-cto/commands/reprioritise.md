---
description: Scan the strategy tree for changes since last review and recommend priority/horizon adjustments
---

# /stratafy-cto:reprioritise

Scan the full strategy tree, detect what changed since the last review, and recommend priority and horizon adjustments. Uses a scan-wide-zoom-narrow approach to stay within context limits.

## When to Use

- Weekly or fortnightly — keeps the strategy tree honest as work progresses
- After a burst of initiative progress — things that were NOW may be done-for-now
- Before quarterly planning — clean up the tree before setting new priorities
- When the NOW horizon feels crowded — too many items competing for attention

## Process

### Step 1: Get User Context & Expert Profile

In parallel:
- Call `get_user_context` with `command_name: "reprioritise"`, `plugin_name: "stratafy-cto"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert` with `role: "cto"` — returns expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step.

### Step 2: Get Tree Shape (Minimal)

Call `get_strategy_tree` with `detail: "minimal"` and no `strategy_id` (full workspace tree). This returns ~2K tokens — just ids, names, status, health scores, and child counts.

Also in parallel:
- `list_key_priorities` — current company priorities for alignment check

### Step 3: Identify What Changed

From the minimal tree, identify items that need attention using these rules:

**Candidates for horizon shift (done-for-now):**
- Initiatives with `status: "in_progress"` AND `completion_percentage >= 70` AND `horizon_phase: "now"`
- These may have remaining work that belongs in a future phase

**Stalled work:**
- Initiatives with `status: "in_progress"` AND `updated_at` older than 14 days
- No recent progress — needs a decision: resume, deprioritise, or mark blocked

**Completed but not closed:**
- Initiatives with `completion_percentage >= 95` AND `status` still `in_progress` or `active`
- Should be marked `completed` or have remaining work split into a follow-up

**New and untriaged:**
- Initiatives with `status: "draft"` AND no `horizon_phase` or `priority` set
- Recently created items that need placement in the tree

**Capacity conflicts:**
- Count initiatives per strategy with `horizon_phase: "now"` AND `status: "in_progress"`
- Flag strategies with more than 4 active NOW initiatives — likely overcommitted

**Health drops:**
- Strategies where `health_status` is `red` or `amber`
- Cross-reference with key priorities to assess strategic impact

### Step 4: Zoom Into Flagged Items

For ONLY the items flagged in Step 3, gather detail. For each flagged initiative, in parallel:
- `get_initiative` — full description, objectives, dates
- `list_objectives` filtered by `initiative_id` — what's done vs remaining

For flagged strategies:
- `get_strategy` — health alerts, content summary

**Do NOT load detail for items that passed all checks — they don't need attention.**

**Do NOT call `get_workspace_snapshot`, `list_risks`, `list_assumptions`, or unfiltered `list_initiatives` — these return oversized payloads.**

### Step 5: Apply Reprioritisation Rules

For each flagged item, determine the recommended action:

**Done-for-now initiatives (>70% complete):**
- Check objectives: are remaining objectives actionable NOW or clearly next-phase?
- If remaining work is next-phase → recommend `horizon_phase: "now" → "next"`
- If remaining work is trivial cleanup → recommend completing the initiative
- If remaining work is actively blocked → recommend logging the blocker

**Stalled initiatives:**
- Check: is there a logged blocker? If yes, surface it — the stall is explained.
- If no blocker: recommend one of:
  - Resume (if it's important and was just forgotten)
  - Deprioritise to NEXT (if other work took precedence)
  - Archive (if the initiative is no longer relevant)

**Capacity conflicts:**
- Recommend which NOW initiatives to shift to NEXT based on:
  - Priority (lower priority shifts first)
  - Dependencies (shift items that are blocked by other NOW work)
  - Strategic alignment (keep items aligned to key priorities)

### Step 6: Present

```
CTO REPRIORITISE — [Date]
Since last review: [date or "first run"]
Scanned: [N] strategies, [N] initiatives

━━━ HORIZON SHIFTS (done-for-now) ━━━━━━━━━━━━━━━━━━━━
◻ [Initiative] ([completion]%) → NOW → NEXT
  Strategy: [strategy name]
  Done: [what's complete]
  Remaining: [what's left — and why it's next-phase]

◻ [Initiative] ([completion]%) → NOW → NEXT
  ...

━━━ READY TO CLOSE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
◻ [Initiative] ([completion]%) → mark completed
  All objectives met. No remaining work.

━━━ STALLED — NEEDS DECISION ━━━━━━━━━━━━━━━━━━━━━━━━━
⚠ [Initiative] — no update in [N] days
  Strategy: [strategy name]
  Options: [resume / deprioritise / archive]
  Recommendation: [pick one with rationale]

━━━ UNTRIAGED ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
◻ [Initiative] — needs priority + horizon
  Strategy: [strategy name]
  Recommendation: [priority] / [horizon] based on [rationale]

━━━ CAPACITY CONFLICTS ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠ [Strategy] has [N] initiatives in NOW (recommend max 4)
  Keep: [list — highest priority, aligned to key priorities]
  Shift to NEXT: [list — lower priority or blocked]

━━━ STRATEGY HEALTH ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔴 [Strategy] — [health score] — [top alert]
🟡 [Strategy] — [health score] — [top alert]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[N] changes recommended

[a] Apply all recommended changes
[r] Review individually
[s] Skip — no changes
```

If there are no findings in a section, omit it entirely.

### Step 7: Apply Changes

If the user approves (all or individually):

For each approved horizon shift:
```
update_initiative(id, horizon_phase: "next")
```

For each approved completion:
```
update_initiative(id, status: "completed", completion_percentage: 100, actual_completion_date: "[today]")
```

For each approved deprioritisation:
```
update_initiative(id, horizon_phase: "next", priority: [adjusted if needed])
```

For each approved triage:
```
update_initiative(id, priority: [recommended], horizon_phase: [recommended])
```

Run all approved mutations in parallel.

After applying, log the review:
```
log_activity(activity_type: "review", entity_type: "workspace", description: "CTO reprioritisation review — [N] changes applied", metadata: { command: "reprioritise", changes_applied: N, changes_skipped: N })
```

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "reprioritise"
- `_change_reasoning`: "[action] — surfaced by reprioritisation review on [date]"

## Rules

1. **Scan wide, zoom narrow.** Always start with `detail: "minimal"` for the full tree. Only load detail for items that fail a check. This keeps context manageable regardless of tree size.
2. **Don't move goal posts.** Shifting an initiative from NOW to NEXT is not the same as deprioritising. The priority stays the same — only the activation phase changes. Make this distinction clear.
3. **Respect the 4-per-strategy rule.** More than 4 active NOW initiatives per strategy means nothing is really prioritised. Be direct about this.
4. **Show your reasoning.** For each recommendation, explain why. The user needs to trust the logic to approve batch changes.
5. **Log the review.** Every run gets logged via `log_activity` so the next run knows when the last review happened.
6. **No silent changes.** Never apply changes without explicit approval. Present, recommend, wait for confirmation.
7. **Omit green.** If a section has no findings, don't show it. The user's time is on the findings, not the all-clear.
8. When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
