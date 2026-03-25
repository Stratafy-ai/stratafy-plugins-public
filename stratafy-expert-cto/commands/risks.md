---
description: Surface technical risks and assumptions across owned strategies
---

# /stratafy-cto:risks

Surface technical risks and assumptions across your owned strategies. Fast scan of what could go wrong and what you're betting on.

## Process

### Step 1: Get User Context & Get Owned Strategies

In parallel:
- Call `get_user_context` with `command_name: "risks"`, `plugin_name: "stratafy-cto"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.
- Call `get_expert_strategies` with `role: "cto"` — returns all strategies this expert owns

### Step 2: Gather Risk Data

From Step 1 results, filter to **active strategies only**.

For each active owned strategy, in parallel:
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — risks linked to this strategy
- `get_assumptions_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — assumptions linked to this strategy

Also in parallel (not per-strategy):
- `get_high_risk_items` with `min_score: 9` — highest severity items across the workspace (may surface unlinked risks relevant to tech)

**Do NOT call `list_risks` or `list_assumptions` without filters — these return oversized payloads.**

### Step 3: Categorise

**Risks by urgency:**
- **Active** — Currently materialising or imminent
- **Watched** — Known, mitigated, under control
- **Latent** — Not yet visible but predictable given current trajectory

**Assumptions by confidence:**
- **Validated** — Tested with evidence
- **Believed** — Team consensus but untested
- **Fragile** — Low confidence, high consequence if wrong

### Step 4: Present

```
CTO RISKS — [Date]

━━━ ACTIVE RISKS ━━━━━━━━━━━━━━━━━━━━━━
🔴 [Risk] — [strategy]
   Severity: [score] | Mitigation: [status]
   Impact if realised: [what breaks]

━━━ WATCHED RISKS ━━━━━━━━━━━━━━━━━━━━━
🟡 [Risk] — [strategy]
   Severity: [score] | Mitigation: [status]

━━━ LATENT RISKS ━━━━━━━━━━━━━━━━━━━━━━
⚪ [Risk pattern] — [what would trigger it]

━━━ FRAGILE ASSUMPTIONS ━━━━━━━━━━━━━━━━
🟡 [Assumption] — confidence [X]%
   If wrong: [consequence for which strategy]
   How to test: [validation approach]

━━━ RECOMMENDATIONS ━━━━━━━━━━━━━━━━━━━━
1. [Risk to mitigate] — [action]
2. [Assumption to test] — [action]
```

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-cto"
- `_source_command`: "risks"
- `_change_reasoning`: brief explanation

## Rules

- **Severity over quantity.** Don't list every risk. Surface the ones that threaten strategic execution.
- **Assumptions are risks too.** An untested assumption with high strategic consequence is more dangerous than a logged risk with mitigation.
- **Be concrete.** "Scalability risk" → "Current DB architecture won't support >10K concurrent users, GTM strategy targets 5K users by Q3."
- When referencing strategies, use markdown links with the `urls.detail` URL from tool responses.
