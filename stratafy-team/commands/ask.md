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

## Process

### Step 1: Log the Question
Call `log_activity` with:
- `activity_type`: `"command_usage"`
- `description`: `"ask"`
- `metadata`: `{ "question": "<the user's question>" }`

### Step 2: Search the Workspace

Run `search_workspace` with:
- `query`: The user's question as-is
- No `entity_types` filter — let semantic similarity decide what's relevant across all types
- `limit`: 10
- Default threshold (0.35)

### Step 3: Assess Results

Check the returned results:
- If **0 results**: Tell the user directly — "Your workspace doesn't contain information that matches this question. This might mean the topic hasn't been documented yet, or the question is outside what's currently captured."
- If **results are returned**: Proceed to answer.

For each result, note the `entity_type`, `name`, `similarity` score, and content. Higher similarity = more relevant.

### Step 4: Answer the Question

Answer the user's question directly, grounded in the workspace content. Follow these rules:

**Grounding**
- Answer ONLY from what the workspace contains. Do not supplement with general knowledge.
- Reference specific entities by name: "Your risk register includes [Risk Name] which..."
- Include entity types so the user knows where information lives: "According to your [strategy/initiative/decision]..."

**Structure**
- Lead with the direct answer in 2-3 sentences
- Then provide supporting detail from the search results
- Group related findings if multiple entity types are relevant

**Critical intelligence**
- If search results include high-severity risks, flag them even if not directly asked
- If results include unvalidated assumptions that bear on the question, surface them
- If results include stale or conflicting information, note it

**Honesty about gaps**
- If the workspace partially answers the question, say what's covered and what's not
- If results are low-similarity (below 0.5), caveat: "These results are tangentially related — the workspace may not directly address this topic"

### Step 5: Log the Outcome

After formulating the answer, log whether the question was successfully answered.

Determine the `answer_quality` based on what happened:
- **`"answered"`** — Search returned relevant results (top result similarity >= 0.5) and you could give a direct, grounded answer
- **`"partial"`** — Search returned results but they were tangential (top similarity < 0.5), or the workspace only partially addressed the question
- **`"unanswered"`** — Zero results, or results were too irrelevant to form a meaningful answer

Call `log_activity` with:
- `activity_type`: `"workspace_question"`
- `description`: `"ask_outcome"`
- `metadata`:
  - `question`: The user's original question
  - `answer_quality`: One of `"answered"`, `"partial"`, `"unanswered"`
  - `result_count`: Number of search results returned
  - `top_similarity`: Similarity score of the best result (or 0 if no results)
  - `entity_types_found`: Array of entity types that appeared in results (e.g. `["strategy", "risk", "assumption"]`)
  - `gap_topic`: (only for `"partial"` or `"unanswered"`) A short label describing what the workspace is missing (e.g. `"pricing strategy"`, `"hiring plan"`, `"competitive positioning"`)

**Why this matters:** Unanswered and partially answered questions are a direct signal about gaps in the workspace. If multiple people ask about pricing and the workspace can't answer, that's a sign the team needs to articulate a pricing strategy. These logs feed into workspace health analysis over time.

### Step 6: Suggest Follow-ups

Based on what you found (or didn't find), suggest one or two next steps:

- If the answer revealed a gap: "Your workspace doesn't have a documented position on [topic]. Want me to help draft one?"
- If risks or assumptions surfaced: "There are [N] risks related to this. Want me to pull the full risk assessment?"
- If the answer connects to a specific strategy: "This relates to your [Strategy Name]. Want me to run a health check on it?"
- If nothing was found: "Want me to help capture your thinking on this topic as an insight or decision?"

## Output Format

```
[Direct answer — 2-3 sentences answering the question]

SOURCES
━━━━━━━
[Entity Type] — [Entity Name]
  [Key detail from this entity relevant to the question]

[Entity Type] — [Entity Name]
  [Key detail]

[Entity Type] — [Entity Name]
  [Key detail]

[If critical intelligence was surfaced:]
⚠️ ALSO WORTH NOTING
  [Risk/assumption/conflict that's relevant even though not asked]

SUGGESTED NEXT STEP
  [One concrete follow-up action]
```

## Rules

- Never answer from general knowledge. If the workspace doesn't have it, say so.
- Keep the answer concise — this is a quick lookup, not a strategy review.
- Reference entities by name so the user can find them in the workspace.
- Surface critical intelligence (high risks, unvalidated assumptions) even if not directly asked.
- Adapt language to the user's role (see role-adaptation skill).
- If the question is conversational rather than a workspace query ("how are you?"), respond naturally without searching.
