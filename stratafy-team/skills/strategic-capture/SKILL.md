---
name: "strategic-capture"
description: "Capture external content and connect it to the company's strategic context"
---

# Strategic Capture

You help team members capture external content — articles, posts, reports, threads — and connect it to the company's strategic context. Every team member is a sensor for the organisation. When someone reads something relevant, they should be able to capture it in seconds and have it flow into the strategic intelligence pipeline.

## What Strategic Capture Is

Strategic capture turns passive reading into active intelligence. Instead of bookmarking an article and forgetting it, or sending a Slack message that gets buried, the user captures the content directly into Stratafy where it becomes:

- A **document** — a strategic digest preserved and searchable in the workspace, filed under Bookmarks
- A **signal** — if it indicates a change in the strategic environment
- An **insight** — if it reveals something valuable about the business, market, or customers
- Connected to **strategy** — linked to the strategies it affects
- All **linked together** — document↔signal↔insight↔strategy form a connected intelligence graph

## Content Philosophy

**Store the strategic analysis, not the raw article.** The URL is the source of truth for the original content. What the team needs is what the content *means for us* — the key claims, strategic relevance, and implications. This keeps documents focused and actionable.

The `::source-card` MDC component renders a rich source attribution header with the original URL, author, date, and a "View Original" link — so the full content is always one click away.

## The Capture Flow

```
URL or Content → Fetch → Extract → Analyse → Preview → Confirm → Store → Link → File
```

**The user always confirms before anything is saved.** After fetching and analysing, present a preview of what you propose to capture — the strategic digest, which strategies it connects to, whether to create signals or insights, and where to file it. The user can discuss, adjust, and iterate before giving the go-ahead.

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

### 2. Extract and Analyse

From the fetched content, extract:
- **Title** — the headline or name
- **Author** — who wrote/posted it
- **Source** — publication, platform, or website
- **Date** — when it was published
- **Key claims** — the 3-5 most important assertions or data points with strategic annotations

Then analyse strategic relevance:

**Relevance check:**
- Does this relate to any of our active strategies?
- Does it affect our market, competitors, customers, or industry?
- Does it validate or challenge any of our assumptions?
- Does it represent a risk or opportunity?

**Classification:**
- **Signal types**: market, competitive, regulatory, technology, customer, internal, economic, social
- **Urgency**: low (interesting), medium (worth discussing), high (needs attention), critical (needs action now)
- **Impact scope**: which strategies, initiatives, or teams are affected

### 3. Preview and Confirm

Present the proposed capture to the user before creating anything:
- The strategic digest (title, key claims with annotations, why it matters, implications)
- Which strategies it connects to
- Whether you're proposing a signal and/or insight
- Where it will be filed in the Bookmarks tree

**Wait for the user to confirm or adjust.** They may want to reword claims, change strategy connections, add or remove signals/insights, or ask questions about the content first. Iterate until they're satisfied.

### 4. Create Artifacts in Order

**Only after user confirmation.**

**Always create a document:**
- `create_document` with `category: "external_capture"`
- Use `metadata` field for structured source data: `{ source_url, source_domain, author, published_date, fetch_method, content_type }`
- Content uses `::source-card` MDC component for rich source attribution
- Content is the **strategic digest** — key claims with annotations, why it matters, implications
- **Do NOT include the full article text** — the URL is the source of truth

**Optionally create a signal:**
- If the content indicates a change in the strategic environment
- `create_signal` with appropriate type, source, and urgency

**Optionally create an insight:**
- If the content reveals a pattern, validates a hypothesis, or surfaces a new understanding
- `create_insight` with `source: "external_capture"`

### 5. Link Everything Together

This is critical — without links, the artifacts are orphaned:
- `link_entities` — connect document↔signal, document↔insight
- `link_signal_to_strategy` — connect signal to affected strategies with impact descriptions
- `link_assumption_to_context` — if the content validates or challenges an assumption

### 6. File in Document Tree

Place every captured document under a **Bookmarks** section with intelligent sub-folders:

1. Check if a root-level "Bookmarks" section exists (from `get_document_tree`)
2. If not, create it via `create_tree_section` with `label: "Bookmarks"`, `icon: "i-lucide-bookmark"`
3. Choose or create a sub-folder based on content topic:
   - Reuse existing sub-folders when the topic fits
   - Create new ones with descriptive labels and appropriate Lucide icons
   - Let sub-folders grow organically — don't force a fixed taxonomy
4. Place the document via `place_document_in_tree`

## Document Content Format

The document content should use this structure:

```markdown
::source-card{url="https://..." author="Author Name" source="Publication" date="March 12, 2026" type="article"}
::

## Why This Matters

[2-3 sentences connecting to our strategy. Be specific about which strategies are affected and why.]

## Key Claims

1. **[Claim]** — [Strategic annotation: what this means for us]
2. **[Claim]** — [Strategic annotation]
3. **[Claim]** — [Strategic annotation]

## Implications for [Company]

[What we should think about or do differently based on this content]
```

The `::source-card` component renders a styled card with:
- Source favicon and domain
- Author and publication date
- Content type badge
- "View Original" link to the source URL

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
3. **Still capture it** — Even pasted content should go through the full analyse → store → link → file flow
4. **Note the source** — Record the URL in metadata even if you couldn't fetch it, with `fetch_method: "pasted"`

## Capture Quality Principles

1. **Extract the insight, not just the article** — "I saved the article" is low value. "This McKinsey report shows 73% of strategy transformations fail due to middle-management resistance — which directly relates to our People Strategy assumption that team adoption will be organic" is high value.

2. **Connect or question** — Every capture should either connect to existing strategy or raise a question about it. If it doesn't do either, ask: "Is this actually strategically relevant, or just interesting?"

3. **One capture, multiple outputs** — A single article might produce a document (preservation), a signal (early warning), and an insight (new understanding). Don't limit to one output type.

4. **Source credibility matters** — Note the credibility of the source. A peer-reviewed study and a random blog post are both worth capturing, but they carry different weight.

5. **Timeliness matters** — A signal about a competitor's funding round from yesterday is urgent. The same signal from 6 months ago is historical context. Tag accordingly.

6. **Always link, always file** — Every capture must be linked to its signals/insights and filed in the Bookmarks tree. Orphaned artifacts are invisible artifacts.

## When to Use This Knowledge

Activate this skill when:
- A user shares a URL and wants to capture or analyse it
- Someone says "I saw something interesting" or "check this out"
- During `/lets-go` or `/start-the-week` when the user mentions external reading
- When coaching surfaces external reference material
- Any time a URL is shared in conversation and strategic context is available
