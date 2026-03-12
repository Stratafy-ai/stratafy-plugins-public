# /stratafy-team:capture

Capture external content — an article, tweet, report, or post — and connect it to your company's strategy. Turns passive reading into strategic intelligence.

## Input

Provide one of:
- **A URL** — "capture https://example.com/article"
- **A URL with context** — "capture https://example.com/article — this is relevant to our GTM strategy"
- **Pasted content** — "capture this: [pasted text]"
- **Multiple URLs** — "capture these: [url1] [url2] [url3]"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "capture"`.

### Step 2: Fetch the Content

**If a URL is provided:**

Check if the content fetch tool is available (`mcp__stratafy__fetch_content`).

If available:
- For auth-walled URLs (linkedin.com, facebook.com, instagram.com): use `force_grok: true`
- For X/Twitter URLs (x.com, twitter.com): use default — Cloudflare renders these successfully
- For all other URLs: use default (Cloudflare Browser Rendering with Grok fallback)

If not available:
- Use `WebFetch` to retrieve the page content
- If WebFetch fails, ask the user to paste the content

**If content was pasted:**
- Skip fetching, proceed directly to analysis

### Step 3: Gather Strategic Context

In parallel with content fetching:
- `get_workspace_snapshot` — Company context and foundation
- `list_strategies` — Active strategies for relevance matching
- `list_key_priorities` — Current focus areas
- `list_assumptions` — Active assumptions to check against

### Step 4: Analyse and Extract

From the content, extract:

```
CONTENT SUMMARY
Title: [extracted title]
Author: [author if available]
Source: [publication/platform]
Date: [publication date if available]
URL: [original URL]

KEY POINTS
1. [Most important claim or data point]
2. [Second most important]
3. [Third most important]

STRATEGIC RELEVANCE
[2-3 sentences: why this matters to the company's strategy]
Connects to: [Strategy/Initiative name(s)]
```

### Step 5: Determine Output Types

Based on the analysis, decide what to create:

**Always create:**
- A **document** via `create_document`
  - `category`: `"external_capture"`
  - `title`: The article/post title
  - `content`: Full extracted text with source attribution
  - `tags`: Relevant strategy areas, content type, source

**Create a signal if:**
- The content indicates a change in the market, competitive landscape, technology, or regulatory environment
- Something new is happening that wasn't happening before
- A competitor made a move, a regulation changed, a trend emerged

**Create an insight if:**
- The content reveals a pattern or validates/challenges a hypothesis
- There's a specific "aha" that's worth preserving beyond the raw content
- A data point changes how you think about an existing strategy or assumption

### Step 6: Store and Connect

Create the artifacts:

```
✅ CAPTURED

📄 Document: "[Title]"
   Source: [author] via [publication], [date]
   [Link to document in Stratafy]

📡 Signal: "[Signal title]" (if created)
   Type: [market/competitive/regulatory/etc.]
   Urgency: [low/medium/high/critical]
   Linked to: [Strategy name]

💡 Insight: "[Insight title]" (if created)
   [One-sentence insight]

🔗 Connected to:
   → [Strategy 1]
   → [Strategy 2 if applicable]
```

### Step 7: Prompt for More

After capturing:
- "Anything else you'd like me to capture?"
- If the content raised questions: "This challenges our assumption that [X]. Want me to flag that for review?"
- If the content suggests action: "This looks like it needs attention from [team/person]. Want me to create a signal with high urgency?"

## Multiple URL Capture

When the user provides multiple URLs:
1. Fetch all URLs in parallel
2. Analyse each independently
3. Look for **patterns across captures** — do they all point to the same trend?
4. Present a summary:

```
✅ BATCH CAPTURE — [N] items

1. "[Title 1]" — [source] → [Strategy]
2. "[Title 2]" — [source] → [Strategy]
3. "[Title 3]" — [source] → [Strategy]

🔍 PATTERN DETECTED (if applicable)
[All three articles point to the same trend: ...]
[Created signal: "..." with high urgency]
```

## Rules

- **Speed matters.** Capture should feel instant. Don't over-analyse — create the document quickly, then offer deeper analysis.
- **Always attribute.** Include the source URL, author, and date. Never strip attribution.
- **Don't over-classify.** Not everything is a signal. Not everything is an insight. Sometimes it's just a useful document to have on file. That's fine.
- **Ask about relevance, don't assume.** If you're unsure which strategy the content relates to, ask: "This looks like it could relate to [Strategy A] or [Strategy B] — which one?"
- **Respect the user's context.** If they said "this is relevant to GTM", don't second-guess them and link it to product strategy instead.
- **Handle failure gracefully.** If you can't fetch the URL, say so immediately and ask for a paste. Don't make the user wait 30 seconds to find out it failed.
- When referencing strategies or documents, use markdown links with the `urls.detail` URL from tool responses.
