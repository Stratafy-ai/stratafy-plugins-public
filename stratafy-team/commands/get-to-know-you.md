# /stratafy-team:get-to-know-you

A 10-minute conversational onboarding session that teaches Stratafy how you think, what you care about, and how you engage with strategy. Not a form — a conversation. Changes everything about how the system works with you afterwards.

## When to Use

- First time using Stratafy in any workspace
- When `get_user_context` shows `needs_onboarding: true`
- When a user wants to reset or update their personal profile
- Before any other skill, if personal context doesn't exist yet

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "get-to-know-you"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context, role, lens, and `_meta.needs_onboarding` flag.

**If `needs_onboarding: false`** (personal context already exists):
Show a summary of what's stored and ask: "Want to update anything, or start fresh?"

**If `needs_onboarding: true`:**
Continue to Step 2.

### Step 2: Set the Frame

Say something like:

> "I'd like to spend about 10 minutes learning how you think and what matters to you. This isn't a profile form — it's a conversation. What you share here changes how I work with you: the questions I ask, the way I frame things, what I lead with. You own this data completely — you can change it any time."

Then move straight into the first question. Don't wait for permission to proceed.

### Step 3: Personal Context (5 questions, conversational)

Ask these one at a time. Wait for each answer before moving on. Listen for lens signals (see Step 5).

**Q1: Goals**
"What's the most important thing you're trying to build or become in the next 12 months?"

→ Store as `personal_goals_12m`

**Q2: Values**
"What do you value most — in work, in people, in yourself? Give me 3-5 words."

→ Store as `values` (array)

**Q3: Energy**
"What tends to give you energy? And what drains you?"

→ Store as `energisers` and `drains`

**Q4: Constraints**
"What's getting in your way right now — externally or internally?"

→ Store as `situational_constraints`

**Q5: Forward Anchor**
"If you could only make one thing true in the next 30 days, what would it be?"

→ Store as `forward_anchor`

**After Q5:** Call `update_personal_context` with all gathered data. Also infer `current_chapter` from the conversation so far and include it.

### Step 4: Role Context (4 questions, workspace-scoped)

**Q6: Mandate**
"Not your job title — what do you actually think your most important job is here?"

→ Store as `mandate`

**Q7: Highest Contribution**
"Where does your time and attention create the most value for this organisation?"

→ Store as `highest_contribution`

**Q8: Success**
"What does success in your role look like in the next 3 months specifically?"

→ Store as `success_3m`

**Q9: Relationship**
"How do you see your relationship to the org — are you building it, running it, advising it, investing in it?"

→ Store as `relationship_to_org`

**After Q9:** Call `update_role_context` with all gathered data.

### Step 5: Lens Inference & Confirmation

Based on the entire conversation, infer the user's primary and secondary lens. Use these signals:

| Lens | Language Signals |
|------|-----------------|
| `builder` | "I'm building...", "how do we do this", action verbs, execution focus |
| `challenger` | "I question...", "does this hold", gap-finding, skeptical framing |
| `operator` | "I run...", "day-to-day", "what needs to happen this week", urgency |
| `advisor` | "In my experience...", "this reminds me of...", pattern-recognition |
| `investor` | "Is this fundable", "what's the return", risk/reward framing |
| `narrator` | "How do we tell this story", audience-awareness, messaging focus |
| `strategist` | "What are we actually choosing", "trade-off", systems thinking |

**Present the inference — don't ask them to choose:**

> "Based on what you've shared, your primary lens is **[lens]** — [plain language description of what that means in their words]. [If secondary:] Underneath that is a **[secondary lens]** — [description]. Does that feel accurate?"

**Design principles for this moment:**
- Show the inference, don't ask a question. "You seem like a builder" beats "which of these describes you?"
- Name the primary first. Secondary is additive.
- Make adjustment easy but not necessary. The default should be to confirm.
- Explain in behaviour, not labels. "You move fast and hold the whole business in your head" is what makes them feel seen.

If they confirm, call `update_user_lens` with the lens values.
If they adjust, use their adjustment.

### Step 6: Complete & Demonstrate

1. Call `complete_personal_onboarding`
2. Show a brief summary of everything captured
3. Demonstrate the difference immediately:

> "Here's how I'll work with you now: [1-2 sentences calibrated to their lens]. For example, [concrete example of how a response would be different]. This is active across every interaction in this workspace."

### Provenance Context

For every mutation, include:
- `_source_plugin`: "stratafy-team"
- `_source_command`: "get-to-know-you"
- `_change_reasoning`: "Personal Intelligence onboarding session — [which part: personal context / role context / lens confirmation]"

## Rules

1. **Conversational, not form-based.** Ask one question at a time. React to answers. Reference what they said earlier.
2. **Short but dense.** Each question should feel effortless but produce rich signal.
3. **Transparent about purpose.** They should know exactly what this changes and why.
4. **Never label without explaining.** The lens types are meaningful to us — to the user, describe the behaviour.
5. **Save as you go.** Don't batch everything to the end. Save personal context after Step 3, role context after Step 4, lens after Step 5.
6. **The lens confirmation is the adoption hook.** Get it right. The moment the system accurately describes how someone thinks is the moment they trust it.
7. **If they want to skip questions**, let them. Partial context is better than no context.
8. **Respect constraints.** If they say they're short on time, compress — but always get to the lens confirmation.
9. **End with a concrete demonstration.** Don't just say "this will be better" — show them HOW their next interaction will be different.
