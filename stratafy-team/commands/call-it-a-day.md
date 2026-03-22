# /stratafy-team:call-it-a-day

End-of-day reflection — wins, blockers, learnings, and alignment. A quick structured wrap-up to close the day with clarity.

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "call-it-a-day"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context and logs the session start.

### Step 2: Gather Context

Call `get_workspace_snapshot` with `sections: ["strategies", "key_priorities"]` and `_source_plugin: "stratafy-team"`.

**Do NOT call `list_strategies` separately** — the snapshot includes the strategy tree.

### Step 3: Reflect

Walk through these questions **one at a time**. Keep each exchange brief — this should take 3-5 minutes total.

#### 3a: Wins
"What went well today?"
- Acknowledge concretely. Connect to strategy if obvious. Don't over-elaborate.

#### 3b: Blockers
"Anything stuck or blocking you?"
- If yes, help brainstorm one unblocking move for tomorrow.
- Only offer to log as a risk if it's a recurring or strategic blocker — not every daily friction.

#### 3c: Learnings
"Learn anything worth remembering?"
- Listen and reflect back. Do NOT auto-create insights.
- Only ask "Want me to capture that as an insight?" if the learning is genuinely strategic or non-obvious. Most daily learnings don't need logging.

#### 3d: Alignment
"Does today's work feel aligned with where you're headed?"
- Brief check, not an analysis. One sentence is fine from the user.

#### 3e: Tomorrow
"What's the one thing for tomorrow?"
- Help them commit to a single priority.

### Step 4: Close

Summarise in 4-5 lines max: wins, blockers, tomorrow's focus. Done.

## Rules

1. **Keep it short.** This is a 3-5 minute wrap-up, not a coaching session.
2. **Do NOT auto-create insights** from casual conversation. Only capture if the user explicitly asks or the learning is genuinely strategic.
3. **Do NOT auto-create risks** from every blocker mentioned. Only offer for recurring or strategic blockers.
4. **One question at a time.** Wait for the answer before moving on.
5. **No lectures.** Reflect back, don't analyse.
6. When referencing a strategy, use markdown links with `urls.detail` URLs.

## Provenance Context

On any mutation the user explicitly requests, include:
- `_source_plugin`: "stratafy-team"
- `_source_command`: "call-it-a-day"
- `_change_reasoning`: brief explanation
