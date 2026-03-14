# /stratafy-cos:decision-log

Capture, track, and review strategic decisions. A Chief of Staff's most important job is ensuring decisions are made, recorded, communicated, and followed through.

## When to Use

- After a meeting where a decision was made
- When a decision needs to be made and you want to structure the process
- Weekly decision review — what's pending, what was decided, what needs follow-up
- When someone asks "why did we decide that?" or "when did we decide that?"

## Input

Provide one of:
- **A decision to log** — "We decided to pause the enterprise rollout until Q3"
- **A decision to make** — "We need to decide whether to hire a VP Sales or promote internally"
- **A review request** — "Show me pending decisions" or "what decisions were made this month?"
- **A lookup** — "Why did we decide to use Supabase?" or "when did we pivot to PLG?"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "decision-log"`.

### Step 2: Determine Mode

**Logging a decision** → Step 3A
**Making a decision** → Step 3B
**Reviewing decisions** → Step 3C
**Looking up a decision** → Step 3D

### Step 3A: Log a Decision

Capture:
1. **What** was decided
2. **Why** — the reasoning and context
3. **Who** made the decision (or which meeting)
4. **When** it was made
5. **Impact** — what changes as a result
6. **Follow-ups** — what actions come from this decision and who owns them

Then:
- `create_decision` with full context, linked to relevant strategy/initiative
- If there are action items, create them as insights or log them

```
✅ DECISION LOGGED

Decision: [What was decided]
Date: [When]
Owner: [Who made/owns it]
Context: [Why — 2-3 sentences]
Strategic link: [Which strategy/initiative this serves]

Follow-ups:
  → [Action 1] — Owner: [who] — By: [when]
  → [Action 2] — Owner: [who] — By: [when]

Communicated to: [who needs to know]
```

### Step 3B: Facilitate a Decision

Structure the decision:

```
🤔 DECISION BRIEF

━━━ THE DECISION ━━━━━━━━━━━━━━━━━━━━━
[What needs to be decided, framed clearly]

━━━ CONTEXT ━━━━━━━━━━━━━━━━━━━━━━━━━━
[Why this decision is needed now]
[What triggered it]
[What happens if we don't decide]

━━━ OPTIONS ━━━━━━━━━━━━━━━━━━━━━━━━━━

Option A: [Name]
  ✅ Pros: [benefits]
  ❌ Cons: [costs/risks]
  📊 Impact: [on strategy, metrics, team]
  ⏱️ Timeline: [how long to implement]

Option B: [Name]
  ✅ Pros: [benefits]
  ❌ Cons: [costs/risks]
  📊 Impact: [on strategy, metrics, team]
  ⏱️ Timeline: [how long to implement]

Option C: [Name / "Do nothing"]
  ✅ Pros: [benefits]
  ❌ Cons: [costs/risks]
  📊 Impact: [on strategy, metrics, team]
  ⏱️ Timeline: [N/A or deadline when this option expires]

━━━ RECOMMENDATION ━━━━━━━━━━━━━━━━━━━
I'd go with Option [X] because [reasoning tied to strategy and priorities].

━━━ ASSUMPTIONS ━━━━━━━━━━━━━━━━━━━━━━
This recommendation assumes:
- [Assumption 1]
- [Assumption 2]
If these are wrong, Option [Y] becomes better.

━━━ REVERSIBILITY ━━━━━━━━━━━━━━━━━━━━
[Is this a one-way door or a two-way door?]
[If one-way: what makes it irreversible?]
[If two-way: what's the cost of reversal?]
```

### Step 3C: Review Decisions

Pull context:
- `list_decisions` — All decisions in the workspace
- `get_pending_decisions` — Decisions awaiting resolution

```
📋 DECISION REVIEW — [Date]

━━━ PENDING DECISIONS ([N]) ━━━━━━━━━━━
⏳ [Decision 1] — Due: [date] — Owner: [who]
   Context: [one line]
   Urgency: [why it can't wait]

⏳ [Decision 2] — Due: [date] — Owner: [who]
   Context: [one line]

━━━ RECENT DECISIONS ([N]) ━━━━━━━━━━━━
✅ [Decision] — [date] — [outcome/status]
   Follow-up status: [complete / in progress / overdue]

✅ [Decision] — [date] — [outcome/status]
   Follow-up status: [complete / in progress / overdue]

━━━ OVERDUE FOLLOW-UPS ━━━━━━━━━━━━━━━
⚠️ [Decision] decided [date] → [Action] was due [date]
   Owner: [who] — Status: [not started / stalled]
```

### Step 3D: Look Up a Decision

- `list_decisions` and search for the relevant decision
- `get_decision` for full detail
- Present the decision with its full context, reasoning, and any follow-up status

## Rules

- **Decisions without follow-ups are wishes.** Every decision should have at least one action item with an owner and a deadline.
- **Record the reasoning, not just the outcome.** "We chose Option A" is useless in 6 months. "We chose Option A because our runway gives us 9 months and the slower approach would take 12" is institutional knowledge.
- **Track reversibility.** One-way doors need more care than two-way doors. Make the distinction explicit.
- **Follow up relentlessly.** A decision that's made but not executed is worse than no decision — it creates a false sense of progress.
- **Surface overdue follow-ups.** This is where a Chief of Staff adds the most value — the accountability layer.
- When referencing decisions or strategies, use markdown links with the `urls.detail` URL from tool responses.
