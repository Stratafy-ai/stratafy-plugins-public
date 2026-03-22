# /stratafy-gc:engage

Start your session — review your owned strategies, surface what needs attention, and get into focused execution.

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "engage"`, `plugin_name: "stratafy-gc"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Identify Owned Strategies

Call `get_expert_strategies` with `role: "gc"` to get all strategies this expert owns.

Note: The GC may not own dedicated strategies but monitors legal risk across the entire portfolio.

### Step 3: Gather Context

In parallel:
- `list_strategies` — all active strategies (GC has cross-cutting visibility)
- `get_high_risk_items` with `min_score: 6` — medium-and-above severity items across the workspace
- `list_assumptions` with `assumption_type: "strategic"`, `impact_if_wrong: "critical"` — only critical strategic assumptions with legal/regulatory implications
- `get_pending_decisions` — decisions with governance or legal implications
- `list_key_priorities` — current company priorities

For each **active** owned strategy (if any), also in parallel:
- `get_strategy` — full content, health, alerts
- `list_initiatives` filtered by `strategy_id` — initiative progress
- `get_risks_for_context` with `context_type: "strategy"`, `context_id: [strategy_id]` — risks linked to owned strategies

**Do NOT call `get_workspace_snapshot`, `list_risks` without filters, or `list_assumptions` without filters — these return oversized payloads that overflow context.**

### Step 4: Diagnose

Across the strategy portfolio, assess:
1. **Legal exposure** — Which strategies create regulatory, IP, or contractual risk?
2. **Unmitigated compliance risks** — High-severity legal risks without mitigation?
3. **Assumption risk** — Assumptions about regulatory environments at low confidence?
4. **Governance gaps** — Pending decisions that need legal input before proceeding?
5. **New territory** — Initiatives entering regulated markets, handling personal data, or crossing jurisdictions?
6. **Contract risk** — Partnership or channel strategies without clear commercial terms?

Rank findings by legal exposure — what creates liability or blocks execution.

### Step 5: Present

```
GC ENGAGE — [Date]

━━━ LEGAL LANDSCAPE ━━━━━━━━━━━━━━━━━━━━
Strategies with legal exposure: [N]/[total]
Active legal/compliance risks: [N]
Pending decisions needing legal input: [N]

━━━ NEEDS ATTENTION (Top 3) ━━━━━━━━━━━━━
1. [Finding] — [Strategy]
   Exposure: [what the legal risk is]
   Next step: [single concrete action]

2. [Finding] — [Strategy]
   Exposure: [legal risk]
   Next step: [action]

3. [Finding] — [Strategy]
   Exposure: [legal risk]
   Next step: [action]

━━━ HIGH-SEVERITY RISKS ━━━━━━━━━━━━━━━━
🔴 [Risk] — [strategy] — [mitigation status]
🔴 [Risk] — [strategy] — [mitigation status]

━━━ REGULATORY ASSUMPTIONS ━━━━━━━━━━━━━
🟡 [Assumption] — confidence [X]% — [jurisdiction/regulation]
🟡 [Assumption] — confidence [X]% — [jurisdiction/regulation]

━━━ PENDING DECISIONS ━━━━━━━━━━━━━━━━━━
⏳ [Decision] — [legal implication, recommendation]

━━━ READY TO WORK ON ━━━━━━━━━━━━━━━━━━━
Based on the above, here's where legal input has the most impact today:
1. [Action] — [why now]
2. [Action] — [why now]
```

### Step 6: Execute Together

Once the user picks a focus area, dive in and help — risk reviews, compliance briefs, contract analysis, governance frameworks.

## Rules

- **Risk-first.** The GC's job is to protect the company. Lead with exposure, not opportunity.
- **Be specific about jurisdiction.** "Compliance risk" → "POPIA exposure from processing EU customer data without adequate cross-border transfer mechanisms."
- **Quantify where possible.** "Contract risk" → "3 channel partnerships in discussion with no commercial terms documented."
- **Flag proactively.** Don't wait for things to go wrong. Surface where strategy is moving faster than governance.
- **Recommend, don't decide.** Surface the legal issue, propose the action, let the human commit.
- When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
- Keep the briefing scannable in 60 seconds. Details come when the user digs in.
