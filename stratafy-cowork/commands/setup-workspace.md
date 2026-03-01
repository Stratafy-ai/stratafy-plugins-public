# /stratafy:setup-workspace

Set up a new Stratafy workspace for a client or organisation. Starts with research, presents a pre-filled draft, then refines and builds — like a strategy consultant who's done their homework.

## When to Use

- Onboarding a new coaching client
- Setting up your own organisation's workspace
- Bootstrapping a workspace from existing strategy documents

## Available Tools

These Stratafy MCP tools are already available — do not search for them:

- **Workspace:** `select_workspace` (supports `domain` search), `create_workspace`, `update_workspace_context`, `get_workspace_snapshot`
- **Foundation:** `update_mission`, `update_vision`, `create_value`, `create_principle`, `create_belief`
- **Strategy:** `create_strategy`, `create_initiative`, `create_objective`, `create_metric`
- **Intelligence:** `create_assumption`, `create_risk`, `create_insight`, `create_signal`, `create_decision`
- **Team:** `create_team`, `add_team_member`

## Input

Ask for just two things:

1. **Company URL** (required) — their website
2. **Company name** (optional) — if it's not obvious from the URL

That's it. Don't ask anything else yet.

If the user also provides session notes, strategy documents, or methodology context, accept those as bonus input — but the URL alone is enough to start.

## Process

### Step 0: Find or Create the Workspace

Before researching, find the right workspace:

1. **Search by domain first.** Call `select_workspace` with the `domain` parameter set to the company URL (e.g. `domain: "globalaccess.co.za"`). This finds any existing workspace matching that domain and auto-selects it.
2. **If a match is found**, call `get_workspace_snapshot` to load existing data. Use this as your starting point — don't re-research what's already populated.
3. **If no match is found**, check the returned workspace list for a name match. If none, you'll create a new workspace during Step 2.

### Step 1: Research

Scrape the company website and pull out everything you can:

- **What they do** — products, services, value proposition
- **Mission / purpose** — any stated mission, about page, founder story
- **Values** — stated values, culture page, how they describe themselves
- **Market** — industry, target customers, competitors mentioned
- **Team** — size, leadership, org structure if visible
- **Products** — product lines, pricing tiers, key features
- **Recent news** — blog posts, press releases, announcements
- **Stage** — startup, growth, enterprise (infer from team size, funding, language)

Use web search to supplement with:

- Crunchbase / LinkedIn data if available
- Recent press coverage
- Industry context

### Step 2: Save Context (BEFORE presenting anything)

**CRITICAL: Save the workspace context BEFORE presenting the foundation draft. This is a hard requirement.**

1. If no workspace exists yet, call `create_workspace` with the company name.
2. Call `update_workspace_context` with everything from research: `industry`, `domain`, and `company_context` as a structured JSON with stage, market position, key products, competitors, revenue, team size, CEO, funding status.
3. **Verify the save succeeded** — check the response. If it failed, fix it before continuing.
4. Only AFTER context is saved, proceed to present the foundation draft.

### Step 3: Present Foundation Draft

Present ONLY the foundation — not strategies, initiatives, or assumptions. Frame it as "here's what I found" not "tell me about your company."

```
FOUNDATION DRAFT — [Company Name]
Based on: [URL] + web research

CONTEXT (saved to workspace)
  Industry: [identified]
  Stage: [startup / growth / scale-up / enterprise]
  Market position: [inferred]
  Key products/services: [listed]

FOUNDATION (needs your confirmation)
  Mission: [drafted from website/about page]
  Vision: [inferred from messaging, or "Not found — we'll define this together"]
  Values:
    1. [Value] — [description]
    2. [Value] — [description]
    ...
  Beliefs: [inferred from how they talk about their market]
  Principles: [any operating principles detected, or "None found — we'll explore this"]

WHAT I COULDN'T FIND
  - [gaps in the foundation that need human input]
```

Do NOT present strategies, assumptions, or risks yet. That comes after the foundation is locked in.

**Ask ONE question with the draft — the most important gap (usually mission or vision). WAIT for the user to respond.**

### Step 4: Confirm & Build Foundation

**IMPORTANT: Ask only ONE question per message.** Do not dump 3-5 questions at once. This is a conversation, not a survey.

**You MUST confirm EVERY foundation element before saving.** Work through them one at a time:

1. Mission — "Is this mission accurate, or has it evolved?"
2. Vision — "What does success look like in 3-5 years?"
3. Values — present the list, ask if they'd keep/change/add
4. Beliefs — present what you inferred, ask if they resonate
5. Principles — "Are there any operating principles or rules the team lives by?" (e.g. "We never ship without testing", "Customer calls get returned same day")

**Do NOT skip beliefs or principles.** If you inferred beliefs, present them and ask. If you found no principles, explicitly ask — many companies have unwritten rules worth capturing.

Once ALL elements are confirmed, **build ALL foundation entities and verify each succeeded**:

1. `update_mission`, `update_vision`
2. `create_value` for EACH value (one call per value)
3. `create_principle` for any operating principles
4. `create_belief` for EACH belief (one call per belief)

**Before moving to Step 5, verify the foundation is complete.** If any create call failed, fix it now.

### Step 5: Pause Before Strategy

After building the foundation, confirm with the user:

"Foundation is complete — mission, vision, [N] values, [N] beliefs, and [N] principles saved. Ready to move on to strategy?"

**WAIT for the user to confirm.** Do NOT immediately present the strategy draft.

### Step 6: Strategy & Execution

Present a strategy draft. Apply the same **one question at a time** rule:

```
STRATEGY DRAFT — [Company Name]

STRATEGY TREE
  Corporate: [inferred overarching direction]
    ├── [Functional strategy 1] — based on [evidence]
    ├── [Functional strategy 2] — based on [evidence]
    └── [Functional strategy 3] — based on [evidence]

LIKELY INITIATIVES
  - [Initiative inferred from evidence]

ASSUMPTIONS YOU'RE PROBABLY MAKING
  - [assumption about their market]

RISKS I'D FLAG
  - [risk based on market / stage / model]
```

Present the draft, then ask ONE question at a time:

- "Is the corporate strategy right — is this the overarching direction?"
- (wait) "Are these the right functional pillars?"
- (wait) "Which initiatives are actually in play?"
- (wait) "Do these assumptions and risks resonate?"

Once confirmed, build:

1. `create_strategy` for corporate and functional strategies
2. `create_initiative` for concrete work linked to strategies
3. `create_assumption` — especially the ones surfaced during verification
4. `create_risk` if identified
5. `create_decision` for choices already made or still open

### Step 7: Summary

Present what was created:

- Total entities created (strategies, initiatives, assumptions, etc.)
- The strategy tree visualised
- What's populated vs. what's still a placeholder
- Suggested next steps: "In your first session, I'd focus on [X] — that's where the gaps are"

## Key Principles

- **Research first, ask second** — never open with "tell me about your company" when you can look it up
- **Save context immediately** — workspace context is factual research data, save it before asking the user anything
- **One question at a time** — this is a conversation, not a survey. Ask, wait, then ask the next
- **Confirm everything** — mission, vision, values, beliefs, AND principles must all be confirmed before saving
- **Pause between phases** — confirm foundation is done before moving to strategy
- **Draft, don't interrogate** — present a hypothesis for them to react to, not a blank form to fill out
- **Surface assumptions** — the most valuable part is making implicit beliefs explicit
- **Good enough to start** — a 70% populated workspace is better than a perfect plan that never ships
- **Use their language** — if the website says "customers" don't say "clients"

## Methodology Adaptations

If the user mentions a methodology, adapt the structure:

**Scaling Up:** Foundation = Core Values + Core Purpose + BHAG. Strategies = Key Thrusts/Capabilities. Initiatives = Rocks. Metrics = Critical Numbers.

**EOS:** Foundation = Core Values + Core Focus. Vision = 10-Year Target + 3-Year Picture. Initiatives = Rocks. Metrics = Scorecard items.

**OKRs:** Strategy pillars become objective containers. Key Results under each. Initiatives support key results.

## Supercharged with Connectors

If you have **Notion** connected, pull existing strategy documents from their Notion workspace.
If you have **Google Calendar** connected, check upcoming planning sessions for timeline context.
If you have **Slack** connected, review relevant channel discussions for strategic context.
If you have **LinkedIn** data via web search, enrich the company profile with team and funding information.
