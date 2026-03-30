---
description: Scan for alignment between stated values and actual decisions
---

# /stratafy-expert-chro:culture-check

Assess whether the company's decisions and priorities are consistent with its stated values and principles. Surfaces culture drift — the gap between what you say and what you do.

## When to Use

- Regular culture health check
- When values feel like wall art rather than operating principles
- After a contentious decision or shift in priorities
- When onboarding and want to understand real vs stated culture

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "culture-check"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Data

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]` — company context
- `list_values` — stated values
- `list_principles` — operating principles
- `list_key_priorities` — current priorities (what the company is actually doing)
- `list_decisions` — recent decisions (how the company is actually deciding)

### Step 3: Assess Alignment

Compare stated culture to actual behaviour:
- **Values-to-decisions alignment** — Do recent decisions reflect stated values? Where are the contradictions?
- **Principles-to-priorities alignment** — Do current priorities follow the operating principles?
- **Consistency check** — Are there values that appear in no decisions or priorities? (dead values)
- **Drift detection** — Are there patterns in decisions that suggest an unstated value has emerged?
- **Tension mapping** — Where do values conflict with each other in practice? (e.g., "move fast" vs "quality first")

### Step 4: Present

```
CULTURE CHECK — [Workspace Name]
[Date]

━━━ VALUES ALIGNMENT SCORE ━━━━━━━━━━━━
[X/10] — [one-line summary]

━━━ LIVING VALUES ━━━━━━━━━━━━━━━━━━━━━
[Values that are clearly reflected in decisions and priorities]
- [Value]: Evidence — [specific decision or priority that reflects it]

━━━ DRIFTING VALUES ━━━━━━━━━━━━━━━━━━━
[Values that are stated but not consistently reflected]
- [Value]: Gap — [where decisions or priorities contradict it]

━━━ DEAD VALUES ━━━━━━━━━━━━━━━━━━━━━━━
[Values with no evidence in recent decisions or priorities]
- [Value]: No recent decisions or priorities reference this value

━━━ EMERGING PATTERNS ━━━━━━━━━━━━━━━━━
[Unstated values that appear to be operating in practice]

━━━ TENSIONS ━━━━━━━━━━━━━━━━━━━━━━━━━━
[Where values conflict with each other in practice]

━━━ RECOMMENDATIONS ━━━━━━━━━━━━━━━━━━━
1. [Specific action to close a values gap]
2. [Specific action to address drift]
3. [Specific action to resolve a tension]
```

### Step 5: Offer Follow-ups

- "Want to update values to reflect reality?" — values refinement
- "Want to check team health?" — `/stratafy-expert-chro:team-health`
- "Want a full org review?" — `/stratafy-expert-chro:org-review`

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "culture-check"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Evidence-based.** Every alignment or drift claim references specific decisions, priorities, or patterns from the workspace data.
2. **No judgement on values themselves.** The CHRO's job is to check alignment, not rewrite values. "Your values say X but your decisions say Y" — the human decides which to change.
3. **Tensions are normal.** Don't treat value tensions as failures. Surface them so the team can make conscious trade-offs.
4. **Dead values are honest signals.** A value nobody acts on is either aspirational (fine) or abandoned (needs updating). Name it.
5. **Culture is what you do, not what you say.** Decisions and priorities are the real culture. Values on a page are intentions.
6. When referencing decisions or values, use markdown links with the `urls.detail` URL from tool responses.
