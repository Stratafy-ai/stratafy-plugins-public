# /stratafy-cto:risks

Surface technical risks and assumptions across your owned strategies. Fast scan of what could go wrong and what you're betting on.

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "risks"`.

### Step 2: Get Owned Strategies

Call `get_expert_strategies` with `_source_plugin: "stratafy-cto"` with the CTO expert ID.

### Step 3: Gather Risk Data

In parallel:
- `list_risks` with `_source_plugin: "stratafy-cto"` — All risks, filter for those linked to owned strategies
- `list_assumptions` with `_source_plugin: "stratafy-cto"` — All assumptions, filter for technical ones
- `get_high_risk_items` with `_source_plugin: "stratafy-cto"` — Highest severity items across the workspace
- `search_workspace` with `_source_plugin: "stratafy-cto"` with query "technical risk security scalability reliability" — surface risks not yet formally logged

### Step 4: Categorise

**Risks by urgency:**
- **Active** — Currently materialising or imminent
- **Watched** — Known, mitigated, under control
- **Latent** — Not yet visible but predictable given current trajectory

**Assumptions by confidence:**
- **Validated** — Tested with evidence
- **Believed** — Team consensus but untested
- **Fragile** — Low confidence, high consequence if wrong

### Step 5: Present

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
