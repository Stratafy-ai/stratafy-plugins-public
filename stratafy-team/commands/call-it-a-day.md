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

As the user answers each question, keep track of structured items for the check-in save at the end.

#### 3a: Wins
"What went well today?"
- Acknowledge concretely. Connect to strategy if obvious. Don't over-elaborate.
- Record as items with `type: "win"`

#### 3b: Blockers
"Anything stuck or blocking you?"
- If yes, help brainstorm one unblocking move for tomorrow.
- Record as items with `type: "blocker"`
- Only offer to log as a risk if it's a recurring or strategic blocker.

#### 3c: Learnings
"Learn anything worth remembering?"
- Listen and reflect back. Do NOT auto-create insights.
- Record as items with `type: "learning"`
- Only ask "Want me to capture that as an insight?" if the learning is genuinely strategic.

#### 3d: Alignment
"Does today's work feel aligned with where you're headed?"
- Brief check, not an analysis.
- If they express concern, record as `type: "concern"`

#### 3e: Tomorrow
"What's the one thing for tomorrow?"
- Help them commit to a single priority.
- This becomes the `tomorrow_priority` field.

### Step 4: Save & Close

**After the reflection is complete**, call `save_check_in` with:
- `session_type`: `"call_it_a_day"`
- `summary`: 2-3 sentence summary of the conversation
- `sentiment`: overall tone (positive/neutral/mixed/negative)
- `alignment_score`: 0-100 based on their alignment answer
- `tomorrow_priority`: what they committed to
- `items`: array of structured items collected during the reflection (wins, blockers, learnings, concerns)

Then present the summary in 4-5 lines: wins, blockers, tomorrow's focus. Done.

## Rules

1. **Keep it short.** This is a 3-5 minute wrap-up, not a coaching session.
2. **Do NOT auto-create insights** from casual conversation. Only capture if the user explicitly asks.
3. **Do NOT auto-create risks** from every blocker mentioned. Only offer for recurring or strategic blockers.
4. **Always save the check-in** at the end — this feeds the team rhythm dashboard.
5. **One question at a time.** Wait for the answer before moving on.
6. **No lectures.** Reflect back, don't analyse.

## Provenance Context

On any mutation the user explicitly requests, include:
- `_source_plugin`: "stratafy-team"
- `_source_command`: "call-it-a-day"
- `_change_reasoning`: brief explanation
