---
description: "Full onboarding — personal intelligence, role setup, strategy orientation, and first action"
---

# /stratafy-team:onboard

The Day 1 command. Onboards a new team member into Stratafy — learns who they are, sets up their role, walks them through the strategies they'll be working on, and gets them into their first work session.

If personal intelligence already exists, skips straight to strategy orientation.

## When to Use

- New team member joining the workspace
- Someone's role has changed and they need re-orientation
- "What should I know about this workspace?"
- First time a user interacts with the stratafy-team plugin

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "onboard"`, `plugin_name: "stratafy-team"`.

Check the response:
- **`needs_onboarding: true`** → Run the full flow from Step 2
- **`needs_onboarding: false`** → Skip to Step 5 (Strategy Orientation). Say: "I already know you — let me walk you through the strategic landscape."

### Step 2: Set the Frame (only if needs_onboarding)

> "Welcome to Stratafy. I'm your execution partner — I connect your daily work to company strategy so you always know not just *what* to do, but *why* it matters."
>
> "We'll spend about 15 minutes: I'll learn how you think, understand your role, walk you through the strategies, and then we'll jump straight into work. Ready?"

Move straight into the first question. Don't wait for permission.

### Step 3: Personal Intelligence (only if needs_onboarding)

Ask these conversationally, one at a time. React to each answer before moving on.

**Q1: Chapter**
"What's the chapter you're in right now — career stage, life stage, what's occupying your mind?"

→ `current_chapter`

**Q2: Values**
"What do you value most — in work, in people, in yourself? Give me 3-5 words."

→ `values` (array)

**Q3: Energy**
"What gives you energy at work? And what drains you?"

→ `energisers`, `drains`

**Q4: Forward Anchor**
"If you could only make one thing true in the next 30 days, what would it be?"

→ `forward_anchor`

**After Q4:** Call `update_personal_context` with all gathered data.

### Step 4: Role Context (only if needs_onboarding)

**Q5: Mandate**
"What's your role here? Not the title — what do you actually think your most important job is?"

→ `mandate`

**Q6: Contribution**
"Where does your time create the most value for this organisation?"

→ `highest_contribution`

**Q7: Success**
"What does success look like for you in the next 3 months?"

→ `success_3m`

**After Q7:** Call `update_role_context` with `mandate`, `highest_contribution`, `success_3m`.

**Lens inference:** Based on the full conversation, infer their primary lens using these signals:

| Lens | Signals |
|------|---------|
| `builder` | Action verbs, execution focus, "how do we do this" |
| `challenger` | Gap-finding, skeptical, "does this hold" |
| `operator` | Day-to-day, urgency, "what needs to happen this week" |
| `advisor` | Pattern-recognition, "in my experience" |
| `investor` | Risk/reward framing, "what's the return" |
| `narrator` | Audience-awareness, messaging focus |
| `strategist` | Trade-offs, systems thinking, "what are we choosing" |

Present the inference — don't ask them to pick from a list:

> "Based on what you've shared, you're a **[lens]** — [plain language description using their own words]. Does that feel right?"

Call `update_user_lens` with their confirmed (or adjusted) lens.
Call `complete_personal_onboarding`.

### Step 5: Strategy Orientation (always runs)

Fetch the workspace context:

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]`
- `list_metrics` — key metrics for the quick health pulse

Present the strategic landscape, adapted to their role:

```
━━━ WELCOME TO [WORKSPACE NAME] ━━━━━━

MISSION: [one line]
VISION: [one line]

━━━ WHAT WE'RE DOING (Active Strategies) ━━━

[Strategy 1]: [health emoji] [health]
  [One sentence: what it is]
  Your connection: [how this relates to their role/mandate]

[Strategy 2]: [health emoji] [health]
  [One sentence: what it is]
  Your connection: [how this relates to their role]

[Strategy N]: ...

━━━ WHERE THE FOCUS IS RIGHT NOW ━━━━━━

1. [Key Priority] — [why it matters]
2. [Key Priority] — [why it matters]
3. [Key Priority] — [why it matters]

━━━ HOW THINGS ARE GOING ━━━━━━━━━━━━━

[2-3 key metrics with traffic light status: 🟢🟡🔴]
```

**The "Your connection" line is critical.** Don't just list strategies — connect each one to what they said about their role, mandate, and success criteria. An engineer hears different things than a marketer about the same strategy. If a strategy has no obvious connection to their role, say so: "This one's not directly in your area, but it affects you because..."

### Step 6: How This Works Day-to-Day

Explain the daily rhythm, adapted to their lens:

> "Here's how this works for you going forward:"
>
> **Starting your day:** `/stratafy-team:lets-go` — I'll connect your tasks to strategy and help you prioritise
>
> **Need advice:** `/stratafy-team:advise-me` — direct recommendation grounded in the actual strategy
>
> **Got a question:** `/stratafy-team:ask` — anything about the workspace, strategies, or how things connect
>
> **End of day:** `/stratafy-team:call-it-a-day` — capture what you accomplished, carry forward what's next
>
> **Update your profile:** `/stratafy-team:get-to-know-you` — if your role, goals, or focus change
>
> "Every interaction is calibrated to you — your lens, your role, your forward anchor."

### Step 7: First Action

End with momentum, not information:

> "Want to jump into your first work session? Tell me what you're working on today and I'll connect it to the strategies we just walked through."

If they engage, transition into a `lets-go` style flow — connect their tasks to strategy, suggest focus order, offer to help execute.

### Provenance Context

For every mutation in this command, include:
- `_source_plugin`: "stratafy-team"
- `_source_command`: "onboard"
- `_change_reasoning`: "Onboarding session — [which part: personal context / role context / lens / strategy orientation]"

## Rules

1. **15 minutes, not 45.** Keep questions tight. React, don't probe endlessly.
2. **Conversational, not a form.** Reference earlier answers. Make it feel like a real conversation.
3. **Connect strategies to their role.** The strategy walkthrough is useless without "why it matters to you" for each one.
4. **Adapt depth to seniority.** Senior leader → strategic picture, trade-offs, portfolio view. Junior team member → "here's what you need to know to do great work."
5. **Skip what's done.** If personal intelligence exists, jump straight to strategy orientation.
6. **End with action.** The best onboarding ends with the person doing something, not just knowing things.
7. **Save as you go.** Personal context after Step 3, role context after Step 4, lens after Step 4. Don't batch to the end.
8. **The lens confirmation is the adoption hook.** The moment the system accurately describes how someone thinks is the moment they trust it. Get it right.
9. When referencing strategies, use markdown links with the `urls.detail` URL from tool responses.
