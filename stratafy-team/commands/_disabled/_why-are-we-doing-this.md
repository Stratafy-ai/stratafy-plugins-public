---
description: "Trace any piece of work up the strategy tree to purpose"
---

# /stratafy-team:why-are-we-doing-this

Trace any piece of work up the strategy tree to purpose. Answers the question every team member eventually asks: "why does this matter?"

## Input

Provide one of:
- **A task or issue** — "why are we doing the API refactor?"
- **An initiative** — "why are we doing the lighthouse program?"
- **A project name** — "why are we doing Project Aurora?"
- **A vague feeling** — "I don't understand why my work matters right now"

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "why-are-we-doing-this"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Find the Work Item

Based on input:
- If it's an initiative name: `list_initiatives` to find it
- If it's a strategy name: `get_strategy` to get full context
- If it's vague: ask the user to describe what they're working on, then search

### Step 3: Trace Up the Strategy Tree

Build the full chain from work to purpose:

```
get_strategy_tree → find the matching initiative → get parent strategy → get parent pillar → connect to mission
```

For each level, pull the detail:
- `get_strategy` for each strategy in the chain
- `get_initiative` for initiative detail
- Foundation summary for mission/vision connection

### Step 4: Present the Chain

```
WHY THIS MATTERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

YOUR WORK
  📌 [Task/issue name]
  [Brief description of what the work involves]
     │
     ▼
INITIATIVE
  🚀 [Initiative name]
  [What this initiative aims to achieve]
  Status: [active/in-progress] | Progress: [X%]
     │
     ▼
STRATEGY
  🎯 [Strategy name]
  [What this strategy is about — the bet we're making]
  Priority: [high/critical] | Type: [core/functional/supporting]
     │
     ▼
STRATEGIC PILLAR
  🏛️ [Parent strategy/pillar name]
  [The big-picture strategic direction]
     │
     ▼
MISSION & VISION
  💫 [Mission — why the company exists]
  🔭 [Vision — where we're headed]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

IN PLAIN LANGUAGE
[One paragraph explaining the chain in natural language, adapted to the person's role]

"You're working on [task] because it's part of [initiative], which is how we execute
[strategy]. That strategy exists because we believe [strategic rationale]. Ultimately,
this connects to our mission to [mission] — your work on [task] is a direct contribution
to [specific outcome]."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

THE METRICS THAT PROVE IT
[Which metrics will move if this work succeeds?]
  📊 [Metric] — currently [value], target [target]
  📊 [Metric] — currently [value], target [target]

If this work succeeds, [metric] should [move in direction] because [causal link].

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5: Handle Edge Cases

**If the chain is broken (task doesn't connect to strategy):**
> "I can't find a clear connection between [task] and the current strategy tree. That doesn't mean it's not important — but it's worth asking: is this serving an implicit strategy that hasn't been documented, or is it legacy work that should be questioned?"

Offer to help: "Want me to suggest which strategy this might belong under?"

**If the work connects to a paused or cancelled strategy:**
> "[Task] traces up to [strategy], which is currently [paused/cancelled]. This work may be orphaned. Worth checking with your lead whether it should continue."

**If the user just feels disconnected:**
Don't jump to the strategy tree. Start with empathy, then ask what they're spending most of their time on. Trace from there.

## Rules

- The "in plain language" paragraph is the most important output. Make it resonate.
- Don't just list the hierarchy — explain the *reasoning* at each level.
- If the chain is strong, reinforce it: "Your work matters because..."
- If the chain is weak, be honest: "The connection isn't clear — let's figure this out."
- Connect to metrics wherever possible — "if this works, this number goes up."
- Adapt language to role: an engineer wants to know the technical "why", a salesperson wants to know the customer "why".
