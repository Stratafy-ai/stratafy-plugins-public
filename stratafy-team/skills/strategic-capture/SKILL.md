# Strategic Capture

You help team members capture external content — articles, posts, reports, threads — and connect it to the company's strategic context. Every team member is a sensor for the organisation. When someone reads something relevant, they should be able to capture it in seconds and have it flow into the strategic intelligence pipeline.

## What Strategic Capture Is

Strategic capture turns passive reading into active intelligence. Instead of bookmarking an article and forgetting it, or sending a Slack message that gets buried, the user captures the content directly into Stratafy where it becomes:

- A **document** — preserved and searchable in the workspace
- A **signal** — if it indicates a change in the strategic environment
- An **insight** — if it reveals something valuable about the business, market, or customers
- Connected to **strategy** — linked to the strategies it affects

## The Capture Flow

```
URL or Content → Fetch → Extract → Analyse → Store → Connect
```

### 1. Fetch the Content

Use `mcp__stratafy__fetch_content` if available. This handles:
- JavaScript-rendered pages (via Cloudflare Browser Rendering API)
- Anti-bot protections
- Auth-walled sources (LinkedIn, Facebook, Instagram — via Grok AI fallback)
- X/Twitter posts and articles render directly via Cloudflare (no Grok needed)

If the content fetch tool is not available, fall back to `WebFetch`. If that also fails, ask the user to paste the content.

**For auth-walled sources** (LinkedIn, Facebook, Instagram):
- Use `force_grok: true` to skip Cloudflare and go directly to AI extraction
- These platforms require authentication and always time out with browser rendering

**Note:** X/Twitter does NOT need `force_grok`. Cloudflare successfully renders posts, articles, and quoted tweets. Only reply threads are auth-walled.

### 2. Extract Key Information

From the fetched content, extract:
- **Title** — the headline or name
- **Author** — who wrote/posted it
- **Source** — publication, platform, or website
- **Date** — when it was published
- **Core content** — the actual text, cleaned of navigation and ads
- **Key claims** — the 3-5 most important assertions or data points

### 3. Analyse Strategic Relevance

This is where capture becomes intelligence. For the extracted content, determine:

**Relevance check:**
- Does this relate to any of our active strategies?
- Does it affect our market, competitors, customers, or industry?
- Does it validate or challenge any of our assumptions?
- Does it represent a risk or opportunity?

**Classification:**
- **Signal types**: market, competitive, regulatory, technology, customer, internal, economic, social
- **Urgency**: low (interesting), medium (worth discussing), high (needs attention), critical (needs action now)
- **Impact scope**: which strategies, initiatives, or teams are affected

### 4. Store in Stratafy

Based on the analysis, create one or more artifacts:

**Always create a document:**
- `create_document` with `category: "external_capture"`
- Include the full extracted content, source URL, author, and date
- Tag with relevant strategy areas

**Optionally create a signal:**
- If the content indicates a change in the strategic environment
- `create_signal` with appropriate type, source, and urgency
- Link to the document for full context

**Optionally create an insight:**
- If the content reveals a pattern, validates a hypothesis, or surfaces a new understanding
- `create_insight` with `source: "external_capture"`
- Include the specific insight, not just "interesting article"

### 5. Connect to Strategy

Link the captured content to relevant strategic elements:
- `link_signal_to_strategy` — if a signal was created
- `link_entities` — connect documents to strategies, initiatives, or objectives
- `link_assumption_to_context` — if the content validates or challenges an assumption

## Role-Adapted Capture

Different roles notice different things. Adapt the analysis:

### Engineering
- Technical blog posts, architecture decisions, open-source announcements
- Focus on: technology signals, build-vs-buy implications, competitive technical capabilities
- Connect to: product strategy, technical initiatives, architecture decisions

### Sales
- Competitor announcements, customer news, market reports, pricing changes
- Focus on: competitive signals, market signals, customer signals
- Connect to: GTM strategy, sales initiatives, customer-facing metrics

### Marketing
- Content trends, competitor campaigns, audience research, brand mentions
- Focus on: market signals, competitive positioning, audience behaviour
- Connect to: brand strategy, content initiatives, marketing metrics

### Leadership
- Industry reports, analyst commentary, regulatory changes, economic indicators
- Focus on: strategic-level signals with cross-functional impact
- Connect to: core strategy pillars, board-level risks, key assumptions

## When Content Fetch Fails

If both the MCP server and WebFetch fail:

1. **Tell the user clearly** — "I couldn't fetch that URL. The site blocks automated access."
2. **Ask for paste** — "Could you paste the content or the key sections you'd like me to capture?"
3. **Still capture it** — Even pasted content should go through the full analyse → store → connect flow
4. **Note the source** — Record the URL even if you couldn't fetch it, so the document links back to the original

## Capture Quality Principles

1. **Extract the insight, not just the article** — "I saved the article" is low value. "This McKinsey report shows 73% of strategy transformations fail due to middle-management resistance — which directly relates to our People Strategy assumption that team adoption will be organic" is high value.

2. **Connect or question** — Every capture should either connect to existing strategy or raise a question about it. If it doesn't do either, ask: "Is this actually strategically relevant, or just interesting?"

3. **One capture, multiple outputs** — A single article might produce a document (preservation), a signal (early warning), and an insight (new understanding). Don't limit to one output type.

4. **Source credibility matters** — Note the credibility of the source. A peer-reviewed study and a random blog post are both worth capturing, but they carry different weight.

5. **Timeliness matters** — A signal about a competitor's funding round from yesterday is urgent. The same signal from 6 months ago is historical context. Tag accordingly.

## When to Use This Knowledge

Activate this skill when:
- A user shares a URL and wants to capture or analyse it
- Someone says "I saw something interesting" or "check this out"
- During `/lets-go` or `/start-the-week` when the user mentions external reading
- When coaching surfaces external reference material
- Any time a URL is shared in conversation and strategic context is available
