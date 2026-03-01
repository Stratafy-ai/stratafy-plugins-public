# Content Patterns

Common MDC composition patterns and anti-patterns for Stratafy content.

## Blog Post Structure

Standard structure for a Stratafy blog post with MDC components:

```mdc
---
title: 'Article Title'
description: 'Meta description under 160 chars'
date: '2026-02-28'
badge:
  label: 'Category'
  color: 'primary'
  variant: 'subtle'
tags:
  - 'Tag1'
  - 'Tag2'
authors:
  - name: 'Author Name'
    description: 'Role'
    avatar:
      src: 'https://...'
    to: 'https://x.com/...'
seo:
  title: 'SEO Title | Stratafy'
  description: 'SEO meta description'
  keywords: 'keyword1, keyword2, keyword3'
hub: 'series-slug'
schemaOrg:
  - type: 'Article'
    headline: 'Article Title'
    datePublished: '2026-02-28'
---

# Article Title

::hub-banner{hub="series-slug"}
::

::note
This article is part of our series on strategy execution. Read the full series for complete context.
::

::tip
**TL;DR:** One or two sentence summary of the key takeaway.
::

**Opening hook paragraph.** Establish the problem or insight.

## Section Heading

Body content with [internal links](/glossary#term) to glossary terms.

::blog-quote{quote="Relevant expert quote that supports your point." author="Expert Name" role="Title, Company"}
::

## Another Section

More content...

::warning
Don't skip the alignment phase. Organizations that rush to execution without alignment see 60% lower success rates.
::

## Conclusion / What This Means

Closing content tying back to the thesis.

::card{title="Next Steps" icon="i-lucide-arrow-right" to="/blog/next-post"}
Continue reading about how to implement continuous alignment in your organization.
::
```

## Common Patterns

### TL;DR Box

Place immediately after the opening, before body content:

```mdc
::tip
**TL;DR:** One-sentence summary of the article's key insight.
::
```

### Series Navigation

For hub posts, place after `<h1>` and before TL;DR:

```mdc
::hub-banner{hub="why-strategies-fail"}
::

::note
This is Part 2 of our series on why strategies fail. Read [Part 1](/blog/part-1) for the foundation.
::
```

### Callout Boxes

Use the semantic callout that matches your intent:

```mdc
::tip
Best practice or recommendation the reader should follow.
::

::note
Additional context or background information.
::

::warning
Something the reader should be careful about.
::

::caution
Critical issue that could cause serious problems if ignored.
::
```

### Callout as Link

Make a callout clickable to navigate somewhere:

```mdc
::tip
---
to: /blog/deep-dive-post
---
For a detailed analysis with supporting research, read our deep dive.
::
```

### FAQ Section

```mdc
## Frequently Asked Questions

::accordion
  ::accordion-item{label="What is strategy execution?"}
  Strategy execution is the process of turning strategic plans into action and results.
  ::

  ::accordion-item{label="Why do strategies fail?"}
  Most strategies fail due to poor communication, lack of alignment, and no feedback loops.
  ::

  ::accordion-item{label="How does continuous alignment help?"}
  Continuous alignment keeps strategy connected to reality through regular check-ins and adaptation.
  ::
::
```

### Expert Quote

```mdc
::blog-quote{quote="The quote text goes here." author="Person Name" role="Title, Company" link="https://x.com/person" avatar="https://ui-avatars.com/api/?name=Person+Name&background=3b82f6&color=fff&size=120"}
::
```

### Step-by-Step Guide

```mdc
::steps

### Step Title
Step description with full markdown support.

### Another Step
More content here.

::
```

### Comparison Tabs (Use Sparingly)

Tabs work for **simple text comparisons only** — no tables, callouts, or nested components inside tab items. If tab content needs any of those, use `### ` subheadings instead.

```mdc
::tabs
  ::tabs-item{label="Traditional" icon="i-lucide-file-text"}
  Annual strategic plans reviewed quarterly. Updates happen in boardrooms.
  ::

  ::tabs-item{label="AI-Native" icon="i-lucide-activity"}
  Living strategy updated continuously. AI agents monitor execution in real-time.
  ::

  ::tabs-item{label="Hybrid" icon="i-lucide-git-merge"}
  Human judgment sets direction. AI handles monitoring and pattern detection.
  ::
::
```

### Feature Cards Grid

```mdc
::card-group
  ::card{title="Real-time Signals" icon="i-lucide-radio" to="/features/signals"}
  Detect strategy drift before it compounds.
  ::

  ::card{title="AI Analysis" icon="i-lucide-brain" to="/features/ai"}
  AI-powered insights from your strategy data.
  ::

  ::card{title="Team Alignment" icon="i-lucide-users" to="/features/alignment"}
  Keep every team member connected to strategy.
  ::
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

### Image with Styling

```mdc
::div{class="flex justify-center my-8"}
![Descriptive alt text for SEO](/images/diagram.png){.rounded-lg .shadow-md}
::
```

### Comparison Table

Standard markdown tables work well:

```mdc
| Traditional | Modern |
| ----------- | ------ |
| Annual reviews | Continuous alignment |
| Top-down only | Bidirectional feedback |
| Static plans | Living strategy |
```

### Collapsible Detail

```mdc
::collapsible{name="research methodology"}
The research methodology involved surveys of 500+ organizations across 12 industries, with longitudinal tracking over 18 months.
::
```

### API/Props Documentation

```mdc
::field-group
  ::field{name="title" type="string" required}
  The document title displayed in the header.
  ::

  ::field{name="status" type="'draft' | 'published' | 'archived'"}
  Current publication status. Defaults to `draft`.
  ::
::
```

## Anti-Patterns

### Wrong: Using `u-` prefix for prose components

```mdc
<!-- BAD: prose components don't use u- prefix -->
::u-alert{color="warning" title="Watch out"}
::

::u-card{variant="outline"}
Content
::
```

Fix: Use prose component names: `::warning`, `::card`

### Wrong: Using YAML items for accordion/tabs

```mdc
<!-- BAD: prose accordion uses slot-based items -->
::accordion
---
items:
  - label: Question
    content: Answer
---
::
```

Fix: Use `::accordion-item` children:

```mdc
::accordion
  ::accordion-item{label="Question"}
  Answer
  ::
::
```

### Wrong: Missing closing delimiter

```mdc
<!-- BAD: no closing :: -->
::card{title="Oops"}
Content here
```

Fix: Always close block components with `::` on its own line.

### Wrong: Slot content in inline components

```mdc
<!-- BAD: inline components cannot have slots -->
:u-button{label="Click"}
#actions
More content
```

Fix: Use block syntax `::` for components that need slots.

### Wrong: PascalCase in MDC

```mdc
<!-- BAD: use kebab-case -->
::BlogQuote{quote="Hi" author="Me"}
::
```

Fix: Always use kebab-case: `::blog-quote`

### Wrong: Multi-line inline props

```mdc
<!-- BAD: inline props must be on a single line -->
::blog-quote{
  quote="Some quote"
  author="Person"
}
::
```

Fix: Keep all `{}` props on a single line, or use YAML `---` blocks for complex data.

### Wrong: Single quotes for string props

```mdc
<!-- BAD: string props use double quotes -->
::card{title='Hello'}
::
```

Fix: Use double quotes for strings: `title="Hello"`

### Wrong: Nesting components inside tabs or cards

```mdc
<!-- BAD: callouts inside tabs-item produce broken rendering -->
::tabs
  ::tabs-item{label="Overview"}
  | Column | Value |
  |--------|-------|
  | A | 1 |
  ::
  ::tabs-item{label="Analysis"}
  ::warning
  This will render with stray :: delimiters.
  ::
  ::
::
```

Fix: Use flat structure with `### ` headings instead of tabs when content includes tables, callouts, or other components:

```mdc
### Overview

| Column | Value |
|--------|-------|
| A | 1 |

### Analysis

::warning
This renders correctly at the top level.
::
```

### Wrong: Using tabs for complex content

```mdc
<!-- BAD: tabs with tables and callouts inside -->
::tabs
  ::tabs-item{label="Metrics"}
  | Metric | Value |
  |--------|-------|
  | Posts | 29 |

  ::caution
  Critical gap identified.
  ::
  ::
::
```

Fix: Only use tabs for simple text comparisons. For anything with tables, callouts, or mixed content, use headings.

### Wrong: No blank line before/after component

```mdc
<!-- BAD: components need breathing room -->
Some text
::tip
Important info
::
More text
```

Fix: Add blank lines around block components for reliable parsing.

## Composition Tips

1. **Don't overuse components** - Plain markdown is more readable. Use components for emphasis, not decoration.
2. **Max 2-3 callouts per section** - Too many callouts dilute their impact.
3. **Place TL;DR at the top** - Readers scan for it immediately.
4. **Use quotes sparingly** - One strong quote per section maximum.
5. **Tables for comparisons** - Prefer markdown tables over complex component layouts.
6. **Internal links in prose** - Use `[term](/glossary#term)` inline, not in separate callout boxes.
7. **CTA at the end** - Close with a `::card` linking to next reading or product.
8. **Match callout semantics** - Use `::tip` for positive advice, `::warning` for caution, `::note` for context.
9. **Keep components flat** - Never nest callouts inside tabs or cards. Use `### ` headings to organize sections instead.
10. **Tabs only for simple text** - Tabs work for short text comparisons (Traditional vs AI-Native). If content needs tables, callouts, or other components, use headings instead.
11. **Headings over tabs for reports** - Weekly reports and documents with mixed content (tables + callouts + analysis) should use `### ` subheadings, not tabs. Tabs are for blog posts with clean A/B comparisons only.
