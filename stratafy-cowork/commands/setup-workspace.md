# /stratafy:setup-workspace

Set up a new Stratafy workspace for a client or organisation. Starts with research, presents a pre-filled draft, then refines and builds — like a strategy consultant who's done their homework.

## When to Use

- Onboarding a new coaching client
- Setting up your own organisation's workspace
- Bootstrapping a workspace from existing strategy documents

## Input

Ask for just two things:
1. **Company URL** (required) — their website
2. **Company name** (optional) — if it's not obvious from the URL

That's it. Don't ask anything else yet.

If the user also provides session notes, strategy documents, or methodology context, accept those as bonus input — but the URL alone is enough to start.

## Process

### Step 0: Load Existing Workspace Context

Before researching, check if the workspace already exists:

- If a workspace is selected or the user mentions one, call `get_workspace_snapshot` to load everything already captured.
- Use existing data as your starting point. Don't re-research what's already populated — focus on gaps and enrichment.
- If the workspace has `workspace_context` set, use it to guide your research.

If no workspace exists yet, skip to Step 1.

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

### Step 2: Present Foundation Draft

Present ONLY the foundation — not strategies, initiatives, or assumptions. Frame it as "here's what I found" not "tell me about your company."

```
FOUNDATION DRAFT — [Company Name]
Based on: [URL] + web research

CONTEXT
  Industry: [identified]
  Stage: [startup / growth / scale-up / enterprise]
  Market position: [inferred]
  Key products/services: [listed]

FOUNDATION
  Mission: [drafted from website/about page]
  Vision: [inferred from messaging, or "Not found — we'll define this together"]
  Values:
    1. [Value] — [description]
    2. [Value] — [description]
    ...
  Beliefs: [inferred from how they talk about their market]

WHAT I COULDN'T FIND
  - [gaps in the foundation that need human input]
```

Do NOT present strategies, assumptions, or risks yet. That comes after the foundation is locked in.

### Step 3: Verify & Build Foundation

Focus entirely on the foundation. Get it confirmed and built before moving on.

Ask targeted refinement questions:

- "I see you say X on your website — is that still your mission, or has it evolved?"
- "Your values page mentions A, B, C. Are those the ones the team actually lives by?"
- "I couldn't find a clear vision statement. What does success look like in 3-5 years?"

Once confirmed (even partially), **build the foundation immediately**:

1. Select or create the workspace
2. `update_workspace_context` with structured company info (industry, stage, market, key products)
3. `update_mission`, `update_vision`
4. `create_value` for each value
5. `create_principle` for any operating principles
6. `create_belief` for beliefs about the market/world

### Step 4: Strategy & Execution

**Only after the foundation is built**, present a separate strategy draft:

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

Refine with the user, then build:

1. `create_strategy` for corporate and functional strategies
2. `create_initiative` for concrete work linked to strategies
3. `create_assumption` — especially the ones surfaced during verification
4. `create_risk` if identified
5. `create_decision` for choices already made or still open

### Step 5: Summary

Present what was created:
- Total entities created (strategies, initiatives, assumptions, etc.)
- The strategy tree visualised
- What's populated vs. what's still a placeholder
- Suggested next steps: "In your first session, I'd focus on [X] — that's where the gaps are"

## Key Principles

- **Research first, ask second** — never open with "tell me about your company" when you can look it up
- **Draft, don't interrogate** — present a hypothesis for them to react to, not a blank form to fill out
- **Surface assumptions** — the most valuable part is making implicit beliefs explicit
- **Good enough to start** — a 70% populated workspace is better than a perfect plan that never ships. They can refine in future sessions
- **Use their language** — if the website says "customers" don't say "clients." If they say "team members" don't say "employees"

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
