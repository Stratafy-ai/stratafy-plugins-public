# Content Patterns

Document patterns and recipes for strategy work in Stratafy workspaces.

## Document Management

### Available Tools

These Stratafy MCP tools manage documents and the document tree:

**Documents:**

- `create_document` — create a new document (title, content, status, category, tags)
- `update_document` — update a document (creates a version snapshot automatically)
- `get_document` — retrieve a document by ID with full content
- `list_documents` — list documents with filtering (status, category, search)
- `search_documents` — search documents by content
- `archive_document` — archive a document

**Document Tree (organising documents into folders):**

- `get_document_tree` — view the full tree hierarchy
- `create_tree_section` — create a section (folder) in the tree
- `place_document_in_tree` — place a document under a section
- `move_tree_node` — move a document or section to a new location
- `update_tree_node` — rename a node or change its icon
- `delete_tree_node` — remove a node (cascades to children if section)
- `get_unplaced_documents` — find documents not yet in the tree

**Linking documents to strategy:**

- `add_document_link` — link a document to a strategy, initiative, objective, decision, assumption, or risk
- `remove_document_link` — remove a link
- `get_document_links` — see what a document is linked to

### Creating a Document

```
1. Create the document with `create_document`
2. Place it in the tree with `place_document_in_tree`
3. Link it to relevant strategy entities with `add_document_link`
```

**Example — creating a strategy review document:**

1. `create_document` with title "Q2 2026 Strategy Review", content in MDC format, status "draft", category "review"
2. `get_document_tree` to see existing sections
3. `place_document_in_tree` under the "Reviews" section
4. `add_document_link` to link it to the corporate strategy

### Organising the Document Tree

A well-organised document tree makes strategy documents easy to find. Recommended structure:

```
Strategy Documents/
  ├── Reviews/
  │   ├── Q1 2026 Strategy Review
  │   └── Q2 2026 Strategy Review
  ├── Decision Briefs/
  │   ├── Pricing Model Decision
  │   └── Market Entry Decision
  ├── Presentations/
  │   ├── Board Briefing — March 2026
  │   └── Investor Update — Q1
  └── Intelligence/
      ├── Competitive Landscape — Feb 2026
      └── Market Analysis — Q1
```

**Creating sections:**

```
create_tree_section with label "Strategy Documents" and icon "i-lucide-folder"
create_tree_section with label "Reviews" and parent_id of "Strategy Documents" section
```

**Moving documents:**

Use `move_tree_node` with the node_id and new parent_id to reorganise. Position is auto-calculated if omitted.

### Finding Unplaced Documents

After creating documents, they may not be in the tree yet. Use `get_unplaced_documents` to find them, then `place_document_in_tree` to organise them.

## Strategy Document Patterns

### Executive Summary

```mdc
# Executive Summary — [Company Name]

::note
Prepared for: Board of Directors | Date: March 2026 | Status: :badge[Draft]
::

## Strategic Direction

Brief overview of the current corporate strategy and key focus areas.

## Key Metrics

::card-group
  ::card{title="ARR" icon="i-lucide-dollar-sign"}
  $1.2M (target: $2M) — :badge[Behind]
  ::

  ::card{title="Retention" icon="i-lucide-heart"}
  94% (target: 90%) — :badge[On Target]
  ::

  ::card{title="Team Size" icon="i-lucide-users"}
  18 (target: 25) — :badge[Behind]
  ::
::

## Top Risks

1. **Key person dependency** — CTO carries critical knowledge with no succession plan
2. **Market timing** — Competitor X may launch before our Q2 release
3. **Cash runway** — 14 months at current burn rate

::warning
Risk #1 has escalated since last review. Recommend immediate action on knowledge transfer.
::

## Decisions Needed

::accordion
  ::accordion-item{label="Pricing model for enterprise tier"}
  Options under consideration: per-seat, usage-based, or hybrid. Decision due by March 15. See Decision Brief for full analysis.
  ::

  ::accordion-item{label="Build vs. buy for analytics engine"}
  Engineering assessment complete. Cost analysis pending. Decision due by April 1.
  ::
::

## Recommended Actions

::steps

### Address key person risk
Initiate knowledge transfer sessions and document critical architecture decisions.

### Accelerate Q2 release
Descope non-essential features to hit the market window before Competitor X.

### Schedule fundraising prep
With 14 months runway, begin investor conversations in Q3.

::
```

### Strategy Review Document

```mdc
# Strategy Review — [Strategy Name]

::note
Review period: Q1 2026 | Reviewer: [Name] | Status: :badge[In Review]
::

## Strategy Overview

**Type:** Functional (Product)
**Status:** Active
**Time Horizon:** 12 months

[Description of the strategy's intent and positioning]

## Execution Scorecard

::metric-card{title="Initiatives On Track" value="4" target="6" icon="i-lucide-check-circle" status="behind"}
::

::metric-card{title="OKR Progress" value="62" unit="%" trend="+8%" icon="i-lucide-target" status="behind"}
::

## Initiative Progress

| Initiative | Status | Owner | Due | Notes |
| --- | --- | --- | --- | --- |
| Coach onboarding flow | :badge[On Track] | Sarah | Q2 | Ahead of schedule |
| API v2 migration | :badge[At Risk] | James | Q1 | Blocked by auth refactor |
| Content engine | :badge[Completed] | Maria | Q1 | Shipped Feb 15 |

## Assumption Health

::tip
2 assumptions validated this quarter — strong evidence for market demand.
::

| Assumption | Confidence | Impact if Wrong | Status |
| --- | --- | --- | --- |
| Mid-market is underserved | Validated | Critical | :badge[Confirmed] |
| CAC decreases with brand | Hypothesis | High | :badge[Untested] |
| Feature parity in 6 months | Likely | High | :badge[On Track] |

## Risk Register

::warning
Risk exposure has increased since last review. Two risks escalated from medium to high.
::

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| Key hire doesn't close | High | High | Parallel sourcing through 3 channels |
| Competitor launches first | Medium | High | Focus on differentiation, not speed |

## Recommendations

1. **Unblock API migration** — resolve auth dependency this sprint
2. **Test CAC assumption** — run a small paid campaign to gather data
3. **Hire backup for key role** — don't rely on single candidate pipeline
```

### Decision Brief

```mdc
# Decision Brief — [Decision Title]

::note
Type: :badge[Type 2 — Reversible] | Status: :badge[Pending] | Due: March 15, 2026
::

## Context

[2-3 paragraphs on why this decision is needed now and what's at stake]

## Options

::tabs
  ::tabs-item{label="Option A" icon="i-lucide-zap"}
  Per-seat pricing at $49/user/month. Simple to explain, predictable revenue, but limits adoption in large organisations.
  ::

  ::tabs-item{label="Option B" icon="i-lucide-activity"}
  Usage-based pricing tied to active workspaces. Aligns with value delivery, but harder to forecast revenue.
  ::

  ::tabs-item{label="Option C" icon="i-lucide-git-merge"}
  Hybrid: base platform fee + per-workspace charge. Balances predictability with value alignment.
  ::
::

## Analysis

| Criterion | Option A | Option B | Option C |
| --- | --- | --- | --- |
| Revenue predictability | High | Low | Medium |
| Adoption friction | Medium | Low | Medium |
| Strategic alignment | Medium | High | High |
| Reversibility | High | Medium | Medium |

## Key Assumptions

::accordion
  ::accordion-item{label="Customers prefer predictable costs"}
  Based on 8 customer interviews. Confidence: likely. If wrong, usage-based becomes more attractive.
  ::

  ::accordion-item{label="Average workspace count is 3-5"}
  Based on beta data. Confidence: validated. Directly affects Option B and C revenue projections.
  ::
::

## Recommendation

::tip
**Recommend Option C (Hybrid).** It balances revenue predictability with value-aligned pricing. The base fee covers operational costs while per-workspace pricing scales with customer value.
::

**Confidence:** Medium — dependent on assumption about customer price sensitivity.

**Key dependency:** Must validate pricing with 5 pilot customers before full rollout.
```

### Board Presentation

```mdc
::::::presentation-deck{title="Board Update — Q1 2026" description="Quarterly strategy review" author="CEO Name" date="March 2026"}

::::presentation-page
# Board Update
## Q1 2026

Company Name
::::

::::presentation-page
# Key Metrics

::metric-card{title="ARR" value="1200000" value-prefix="$" target="2000000" trend="+18%" status="behind"}
::

::metric-card{title="Customers" value="47" target="100" trend="+12" status="behind"}
::

::metric-card{title="NPS" value="72" trend="+8" status="on-target"}
::

::speaker-notes
ARR growth rate is accelerating — 18% QoQ. Behind target but trajectory is positive. NPS is a strength — customers love the product.
::
::::

::::presentation-page
# Strategic Priorities

::card-group
  ::card{title="Growth" icon="i-lucide-trending-up"}
  Expand mid-market. Target: 100 customers by year-end.
  ::

  ::card{title="Product" icon="i-lucide-box"}
  AI-forward features. Ship radar and alignment scans.
  ::

  ::card{title="People" icon="i-lucide-users"}
  Scale team to 25. Key hires: VP Sales, Senior Engineers.
  ::
::

::speaker-notes
Three pillars unchanged from last quarter. Consistency is intentional — we're executing, not pivoting.
::
::::

::::presentation-page
# Risks & Decisions

::warning
Key person risk has escalated. Knowledge transfer plan needed.
::

**Pending decisions:**
- Enterprise pricing model (due: March 15)
- Series A timeline (due: Q2)

::speaker-notes
Board input needed on fundraising timeline. 14 months runway gives us flexibility but shouldn't wait too long.
::
::::

::::presentation-page
# Roadmap

::project-timeline
---
title: 2026 Roadmap
items:
  - date: Q1
    title: Foundation
    status: completed
    description: Core platform, first 30 customers
  - date: Q2
    title: Growth
    status: in_progress
    description: Mid-market push, 50 customers
  - date: Q3
    title: Scale
    status: pending
    description: Channel partnerships, 75 customers
  - date: Q4
    title: Expand
    status: pending
    description: Enterprise tier, 100 customers
---
::
::::

::::::
```

## Common Patterns

### Callout Boxes

Use the semantic callout that matches your intent:

```mdc
::tip
Best practice or recommendation to follow.
::

::note
Additional context or background information.
::

::warning
Something to watch out for — a risk or concern.
::

::caution
Critical issue that could cause serious problems if ignored.
::
```

### Expandable Detail Sections

For supporting detail that some readers need and others don't:

```mdc
::accordion
  ::accordion-item{label="Supporting Evidence"}
  Detailed research, data, or methodology that backs up the main argument.
  ::

  ::accordion-item{label="Historical Context"}
  What happened previously and how it informs the current recommendation.
  ::
::
```

### Step-by-Step Process

```mdc
::steps

### Step Title
Step description with full markdown support.

### Another Step
More content here.

::
```

### Metric Dashboard

```mdc
::metric-card{title="Revenue" value="250000" value-prefix="$" trend="+12%" status="on-target"}
::

::metric-card{title="Churn" value="4.2" unit="%" trend="-0.8%" status="ahead"}
::
```

### Implementation Timeline

```mdc
::project-timeline
---
title: Rollout Plan
items:
  - date: Week 1
    title: Setup
    status: completed
  - date: Week 2
    title: Integration
    status: in_progress
  - date: Week 3
    title: Launch
    status: pending
---
::
```

### Comparison Table

Standard markdown tables work well for comparisons:

```mdc
| Current State | Target State |
| --- | --- |
| Annual reviews | Continuous alignment |
| Top-down only | Bidirectional feedback |
| Static plans | Living strategy |
```

### Collapsible Detail

```mdc
::collapsible{name="research methodology"}
Detailed methodology description that most readers can skip.
::
```

## Anti-Patterns

### Wrong: Using YAML items for accordion/tabs

```mdc
<!-- BAD -->
::accordion
---
items:
  - label: Question
    content: Answer
---
::
```

Fix: Use `::accordion-item` children instead.

### Wrong: Missing closing delimiter

```mdc
<!-- BAD: no closing :: -->
::card{title="Oops"}
Content here
```

Fix: Always close block components with `::` on its own line.

### Wrong: Multi-line inline props

```mdc
<!-- BAD: props must be on a single line -->
::metric-card{
  title="Revenue"
  value="250000"
}
::
```

Fix: Keep all `{}` props on a single line, or use YAML `---` blocks for complex data.

### Wrong: Single quotes for string props

```mdc
<!-- BAD -->
::card{title='Hello'}
::
```

Fix: Use double quotes: `title="Hello"`

### Wrong: Nesting components inside tabs or cards

```mdc
<!-- BAD: callouts inside tabs-item break rendering -->
::tabs
  ::tabs-item{label="Analysis"}
  ::warning
  This will render with stray :: delimiters.
  ::
  ::
::
```

Fix: Use `### ` headings instead of tabs when content includes callouts, tables, or other components.

### Wrong: No blank line before/after component

```mdc
<!-- BAD -->
Some text
::tip
Important info
::
More text
```

Fix: Add blank lines around block components for reliable parsing.

## Composition Tips

1. **Don't overuse components** — plain markdown is more readable. Use components for emphasis, not decoration.
2. **Max 2-3 callouts per section** — too many callouts dilute their impact.
3. **Tables for comparisons** — prefer markdown tables over complex component layouts.
4. **Match callout semantics** — `::tip` for positive advice, `::warning` for risks, `::note` for context.
5. **Keep components flat** — never nest callouts inside tabs or cards. Use `### ` headings to organise sections.
6. **Headings over tabs for reports** — strategy reviews and documents with mixed content should use `### ` subheadings, not tabs.
7. **Link documents to strategy** — always use `add_document_link` to connect documents to the strategies, initiatives, or decisions they relate to. This makes them discoverable from the strategy tree.
8. **Place documents in the tree** — after creating a document, place it in the document tree so it's organised and easy to find.
