---
description: "Get a strategy summary from the FD's perspective, tailored to who you are"
---

# /stratafy-expert-cfo:summary

Get a summary of the FD's owned strategies — what they are, how they're tracking, and what matters most right now. Framed through a financial lens and calibrated to your role and context.

## When to Use

- "What's the FD responsible for?"
- "Give me the finance picture"
- When a team member wants to understand the financial strategy without diving into details
- When leadership wants a quick financial status read
- When onboarding someone who needs to understand the financial landscape

## Process

### Step 1: Get User Context & Owned Strategies

In parallel:
- Call `get_user_context` with `command_name: "summary"`, `plugin_name: "stratafy-expert-cfo"`.
  This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start.
- Call `get_expert` with `role: "fd"` — returns the FD expert profile including its `id`

Then call `get_expert_strategies` with the `expert_id` from the previous step — returns all strategies this expert owns with name, status, health, and strategy type.

### Step 2: Summarise Through the Financial Lens

For each owned strategy, produce a concise summary that covers:
- **What it is** — one sentence on the strategic intent
- **Status** — health score and current state
- **Financial significance** — why this strategy matters financially (runway, revenue, cost structure, funding readiness)

Then synthesise across all strategies into an overall financial posture statement.

### Step 3: Adapt to the User

Use the user context from Step 1 to calibrate depth and framing:

**If the user is a peer (CFO, FD, finance executive):**
- Executive-level, no hand-holding
- Lead with the posture statement, then strategy-by-strategy
- Flag strategic tensions and trade-offs between strategies
- Reference their forward anchor if financially relevant

**If the user is a founder/CEO:**
- Focus on runway implications and investor readiness
- Connect financial strategies to company-level priorities
- Flag what needs their decision vs what's handled

**If the user is a team member:**
- Explain what each strategy means for their work
- Use plain language — no jargon without explanation
- Connect to their forward anchor: "Given you're focused on [X], the strategy that affects you most is..."

**If the user's role is unknown:**
- Start with a clear, accessible summary
- Offer to go deeper on any strategy

### Step 4: Present

```
FD SUMMARY — [Date]

━━━ FINANCIAL POSTURE ━━━━━━━━━━━━━━━━━━
[2-3 sentences: overall financial position, biggest opportunity, biggest risk]

━━━ OWNED STRATEGIES ([N] active) ━━━━━━

[Strategy 1]: [health emoji] [health score]
  [One sentence: what it is and why it matters financially]
  Status: [current state in plain language]

[Strategy 2]: [health emoji] [health score]
  [One sentence: what it is and why it matters financially]
  Status: [current state in plain language]

[Strategy N]: ...

━━━ THE HEADLINE ━━━━━━━━━━━━━━━━━━━━━━
[One paragraph: the financial story these strategies tell together.
 What's working, what's under pressure, what to watch.]
```

### Step 5: Offer Next Steps

Based on the summary, offer relevant follow-ups:
- "Want me to dive deeper into any of these?" → engage or lets-go
- "Want to see the metrics behind this?" → analyse-metrics
- "Want to check financial alignment?" → financial-scan
- "Preparing for investors?" → investor-prep

### Provenance Context

For every mutation in this command, include:
- `_source_plugin`: "stratafy-expert-cfo"
- `_source_command`: "summary"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **This is a read-only command.** Summarise, don't modify. No creating insights, risks, or decisions here.
2. **Financial lens always.** Every strategy gets a financial "so what" — even if the strategy isn't inherently financial.
3. **Adapt to the audience.** A junior team member and a CFO should get meaningfully different summaries of the same data.
4. **One screen.** The summary should be scannable in under 30 seconds. If someone wants depth, they'll ask.
5. **Honest.** If a strategy is struggling, say so. If the financial picture is unclear, say that too.
6. **Connect to forward anchor.** If the user has one, reference it naturally — don't force it.
7. When referencing strategies, use markdown links with the `urls.detail` URL from tool responses.
