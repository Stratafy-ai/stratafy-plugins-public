---
description: "Ask any question and get an answer grounded in your live strategy workspace"
---

# /stratafy-team:ask

Ask any question and get an answer grounded in your live strategy workspace. Claude searches across all workspace entities — strategies, initiatives, decisions, risks, assumptions, insights, metrics, objectives, signals, and documents — and answers from what the workspace actually contains.

## Input

Any natural language question:
- "What are our biggest risks heading into the fundraise?"
- "What's our position on pricing?"
- "What assumptions are we making about the enterprise market?"
- "What metrics should I watch for the GTM strategy?"
- "What decisions are pending around hiring?"
- "What did we decide about the partnership approach?"
- "What are our key priorities?"
- "Show me our risks"

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "ask"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Classify and Route the Question

Before searching, classify the question into a retrieval strategy. This determines which tools to call — and they should run **in parallel** where possible.

#### Strategy A: Structured Listing
**Trigger:** The question asks to list or enumerate a specific entity type.
- "What are our risks?" / "Show me our assumptions" / "List our key priorities" / "What metrics do we track?"

**Action:** Call BOTH in parallel:
1. The dedicated list tool for the entity type
2. `search_workspace` with the question (for cross-entity context)

Entity type → list tool mapping:

| Question pattern | List tool | Notes |
| --- | --- | --- |
| risks | `list_risks` | |
| assumptions | `list_assumptions` | |
| key priorities | `list_key_priorities` | |
| metrics | `list_metrics` | |
| objectives | `list_objectives` | |
| strategies | `list_strategies` | |
| initiatives | `list_initiatives` | |
| decisions | `list_decisions` | |
| signals | `list_signals` | |
| insights | `list_insights` | |
| feedback | `list_feedback` | |
| reviews | `list_reviews` | |
| values | `list_values` | |
| principles | `list_principles` | |
| beliefs | `list_beliefs` | |

**Presentation:** Lead with the structured data in a scannable format (table or numbered list), then add cross-entity context from the semantic search below it.

#### Strategy B: Targeted Lookup
**Trigger:** The question asks about a specific topic, entity, or concept.
- "What's our position on pricing?" / "Tell me about the GTM strategy" / "What did we decide about hiring?"

**Action:** Run `search_workspace` only, with:
- `query`: The user's question as-is
- No `entity_types` filter
- `limit`: 10
- Default threshold (0.35)

#### Strategy C: Cross-Cutting Analysis
**Trigger:** The question is broad or asks for synthesis across multiple dimensions.
- "What should I worry about?" / "What's changed recently?" / "What are we missing?"

**Action:** Run `search_workspace` with the question, PLUS call supplementary tools in parallel where useful:
- "What should I worry about?" → also call `get_high_risk_items` and `get_pending_decisions`
- "What's our execution health?" → also call `list_objectives` and `list_key_priorities`

### Step 3: Assess Results

Evaluate result quality using calibrated similarity thresholds:
- **Strong match** (similarity >= 0.45): High confidence — the workspace directly addresses this topic
- **Moderate match** (similarity 0.38–0.45): Relevant context exists but may not directly answer the question
- **Weak match** (similarity < 0.38): Tangential — caveat with "These results are related but may not directly address your question"

If **0 results** from all sources: Tell the user directly — "Your workspace doesn't contain information that matches this question. This might mean the topic hasn't been documented yet."

For each result, note the `entity_type`, `entity_id`, `title`, `similarity` score, and content.

### Step 4: Answer the Question

Answer the user's question directly, grounded in the workspace content. Follow these rules:

**Grounding**
- Answer ONLY from what the workspace contains. Do not supplement with general knowledge.
- Reference specific entities by name with hyperlinks: "Your risk register includes [Risk Name](url) which..."
- Include entity types so the user knows where information lives: "According to your [strategy/initiative/decision]..."

**Structure**
- For **Strategy A** (listings): Present entities in a scannable format — table or numbered list with key fields. Then add any cross-entity context from the semantic search.
- For **Strategy B** (targeted): Lead with the direct answer in 2-3 sentences, then provide supporting detail from the search results grouped by entity type.
- For **Strategy C** (cross-cutting): Synthesise across the different data sources into a coherent answer with sections.

**Critical intelligence**
- If search results include high-severity risks, flag them even if not directly asked
- If results include unvalidated assumptions that bear on the question, surface them
- If results include stale or conflicting information, note it

**Honesty about gaps**
- If the workspace partially answers the question, say what's covered and what's not
- If all results are weak matches (below 0.38), caveat accordingly

### Step 5: Log the Outcome

After formulating the answer, log whether the question was successfully answered.

Determine the `answer_quality` based on what happened:
- **`"answered"`** — Relevant results found (top similarity >= 0.45 or structured list returned data) and you could give a direct, grounded answer
- **`"partial"`** — Results were moderate (top similarity 0.38–0.45), or the workspace only partially addressed the question
- **`"unanswered"`** — Zero results, or results were too weak to form a meaningful answer

Call `log_activity` with:
- `activity_type`: `"workspace_question"`
- `description`: `"ask_outcome"`
- `metadata`:
  - `question`: The user's original question
  - `answer_quality`: One of `"answered"`, `"partial"`, `"unanswered"`
  - `retrieval_strategy`: One of `"structured_listing"`, `"targeted_lookup"`, `"cross_cutting"`
  - `result_count`: Number of search results returned
  - `top_similarity`: Similarity score of the best result (or 0 if no results)
  - `entity_types_found`: Array of entity types that appeared in results (e.g. `["strategy", "risk", "assumption"]`)
  - `gap_topic`: (only for `"partial"` or `"unanswered"`) A short label describing what the workspace is missing (e.g. `"pricing strategy"`, `"hiring plan"`, `"competitive positioning"`)

**Why this matters:** Unanswered and partially answered questions are a direct signal about gaps in the workspace. If multiple people ask about pricing and the workspace can't answer, that's a sign the team needs to articulate a pricing strategy. These logs feed into workspace health analysis over time.

### Step 6: Suggest Follow-ups

Based on what you found (or didn't find), suggest one or two next steps:

- If the answer revealed a gap: "Your workspace doesn't have a documented position on [topic]. Want me to help draft one?"
- If risks or assumptions surfaced: "There are [N] risks related to this. Want me to pull the full risk assessment?"
- If the answer connects to a specific strategy: "This relates to your [Strategy Name](url). Want me to run a health check on it?"
- If nothing was found: "Want me to help capture your thinking on this topic as an insight or decision?"

## Entity URL Construction

Every entity in the search results has an `entity_type` and `entity_id`. Construct clickable links using this pattern:

```
https://stratafy.ai/ws/{workspace_id}/{path}
```

Where `{workspace_id}` is the current workspace ID (from `select_workspace` or session context) and `{path}` is:

| Entity Type   | URL Path                          |
| ------------- | --------------------------------- |
| `strategy`    | `strategy/{entity_id}`            |
| `initiative`  | `initiatives/{entity_id}`         |
| `objective`   | `objectives/{entity_id}`          |
| `metric`      | `metric/{entity_id}`              |
| `decision`    | `decisions/{entity_id}`           |
| `insight`     | `cortex/insights/{entity_id}`     |
| `signal`      | `cortex/signals/{entity_id}`      |
| `assumption`  | `assumptions/{entity_id}`         |
| `risk`        | `risks/{entity_id}`               |
| `document`    | `documents/{entity_id}`           |
| `plan`        | `plans/{entity_id}`               |

For foundation entities (if they appear):

| Entity Type   | URL Path                          |
| ------------- | --------------------------------- |
| `mission`     | `foundation/mission`              |
| `vision`      | `foundation/vision`               |
| `value`       | `foundation/values/{entity_id}`   |
| `principle`   | `foundation/principles/{entity_id}` |
| `belief`      | `foundation/beliefs/{entity_id}`  |

## Output Format

### For Structured Listings (Strategy A)

```
Here are your [entity type plural]:

| # | Name | Status | [Key Field] |
|---|------|--------|-------------|
| 1 | [Entity Name](url) | active | [value] |
| 2 | [Entity Name](url) | active | [value] |

[If semantic search found cross-entity context:]
RELATED CONTEXT
  [Insight/strategy/decision that connects to these entities]

SUGGESTED NEXT STEP
  [One concrete follow-up action]
```

### For Targeted Lookups and Cross-Cutting Analysis (Strategy B & C)

```
[Direct answer — 2-3 sentences answering the question]

SOURCES
━━━━━━━
[Entity Type] — [Entity Name](https://stratafy.ai/ws/{workspace_id}/{path})
  [Key detail from this entity relevant to the question]

[Entity Type] — [Entity Name](https://stratafy.ai/ws/{workspace_id}/{path})
  [Key detail]

[Entity Type] — [Entity Name](https://stratafy.ai/ws/{workspace_id}/{path})
  [Key detail]

[If critical intelligence was surfaced:]
⚠️ ALSO WORTH NOTING
  [Risk/assumption/conflict that's relevant even though not asked]

SUGGESTED NEXT STEP
  [One concrete follow-up action]
```

Every entity name in the response MUST be a markdown hyperlink to the entity in Stratafy. This applies to output tables, the SOURCES section, and any entity referenced in the answer text itself.

## Rules

- **Pass the user's question directly to `search_workspace` as-is.** Do not rephrase, summarise, extract keywords, or "improve" the query. The vector search works on natural language — send exactly what the user typed.
- Never answer from general knowledge. If the workspace doesn't have it, say so.
- Keep the answer concise — this is a quick lookup, not a strategy review.
- Reference entities by name with hyperlinks so the user can click through to Stratafy.
- Surface critical intelligence (high risks, unvalidated assumptions) even if not directly asked.
- Adapt language to the user's role (see role-adaptation skill).
- If the question is conversational rather than a workspace query ("how are you?"), respond naturally without searching.
- Always run tools in parallel where there are no dependencies between them.
