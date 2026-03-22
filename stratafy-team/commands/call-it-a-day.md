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

Walk through these questions **one at a time**. Keep each exchange brief.

As the user answers each question, keep track of structured items for the check-in.

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

### Step 4: Present Summary

After all reflection questions, present a summary:

```
━━━ TODAY'S WRAP-UP ━━━━━━━━━━━━━━━━━━
Wins: [brief list]
Blockers: [if any, or "Clean day"]
Learning: [if any]
Alignment: [score and one-liner]
Tomorrow: [the one thing]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Then ask: **"Anything you want to adjust or talk through before I save this?"**

Wait for the user's response. If they want to discuss something, explore it. If they're good, move to Step 5.

### Step 5: Save

Call `save_check_in` with:
- `session_type`: `"call_it_a_day"`
- `summary`: 2-3 sentence summary of the conversation
- `sentiment`: overall tone (positive/neutral/mixed/negative)
- `alignment_score`: 0-100 based on their alignment answer
- `tomorrow_priority`: what they committed to
- `items`: array of structured items collected during the reflection (wins, blockers, learnings, concerns)

Confirm it's saved.

### Step 6: Open the Floor

After saving, ask: **"Anything else on your mind before you switch off? Happy to dig into anything that came up."**

If the user wants to discuss something — a blocker, a strategic concern, a decision they're mulling — engage as a coach. Keep it conversational and use the user's lens (builder → action-oriented, strategist → trade-off-focused, etc.).

If they're done, close warmly and briefly. No long sign-offs.

## Rules

1. **Do NOT auto-create insights** from casual conversation. Only capture if the user explicitly asks.
2. **Do NOT auto-create risks** from every blocker mentioned. Only offer for recurring or strategic blockers.
3. **Always save the check-in** — but only after presenting the summary and giving the user a chance to adjust.
4. **One question at a time.** Wait for the answer before moving on.
5. **No lectures.** Reflect back, don't analyse.
6. **The open floor is optional.** If the user says they're done, respect it. Don't push.

## Provenance Context

On any mutation the user explicitly requests, include:
- `_source_plugin`: "stratafy-team"
- `_source_command`: "call-it-a-day"
- `_change_reasoning`: brief explanation
