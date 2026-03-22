# /stratafy-team:call-it-a-day

End-of-day reflection — wins, blockers, learnings, and alignment. Walk through a structured 5-step process to close out the day with clarity.

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "call-it-a-day"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` — Company context and strategy overview
- `list_strategies` — Strategy tree for alignment check

### Step 3: Walk Through Reflection

Go through these 5 steps **one at a time**. Ask one question, wait for the answer, then move to the next.

#### 3a: Wins
"What went well today? What did you accomplish or make progress on?"
- Celebrate concrete progress
- Connect wins to strategic initiatives when possible
- Adapt framing to role (see role-adaptation skill)

#### 3b: Blockers
"What got in your way today? Anything still stuck?"
- If the user names a blocker, offer to log it: `create_risk` with appropriate risk type
- Help brainstorm unblocking moves for tomorrow

#### 3c: Learnings
"Did you learn anything today — about the work, the team, or the strategy?"
- When the user shares a learning, **proactively log it** as an insight (don't just offer — do it)
- `create_insight` with `source: "execution_session"`, appropriate category, and tags including `"end_of_day"`

#### 3d: Alignment Check
"Looking at the current strategy, does today's work feel aligned? Anything feel off?"
- Reference the strategy tree — help them notice drift early
- If they surface a strategic observation, log it as an insight
- Adapt the check to their role: an engineer checks product alignment, a salesperson checks GTM alignment

#### 3e: Tomorrow Setup
"What's the single most important thing for tomorrow?"
- Help them commit to one priority
- Connect it to an active initiative if possible
- "If you only get one thing done tomorrow, make it [X]"

## Insight Logging

### Provenance Context
For every mutation (create_insight, create_risk), include:
- `_source_plugin`: "stratafy-team"
- `_source_command`: "call-it-a-day"
- `_change_reasoning`: Brief explanation tied to the reflection (e.g. "End-of-day reflection surfaced alignment concern with GTM strategy")

As reflection insights emerge during **any step**, proactively call `create_insight`:
- **source**: `"execution_session"`
- **category**: Most appropriate from: `"strategic"`, `"operational"`, `"product"`, `"market"`, `"team"`, `"technical"`
- **tags**: Relevant tags plus always include `"end_of_day"`
- **summary**: Clear, concise statement of the learning or observation

**What counts as an insight:**
- Learnings about what worked or didn't
- Patterns noticed across work or team dynamics
- Strategic observations or alignment realisations
- Unexpected discoveries or market signals

**What is NOT an insight (do not log):**
- Mechanical events (start/end times, task counts)
- Simple status updates ("I finished task X")
- Tomorrow's plan — that's planning, not learning

## Rules

1. Go through steps **in order**, one at a time.
2. Be warm but not sappy. Respect their time.
3. Proactively log insights — don't just offer to.
4. Proactively offer to log blockers as risks.
5. Keep the whole reflection to ~5 minutes of conversation.
6. When referencing a strategy, use markdown links with `urls.detail` URLs.
