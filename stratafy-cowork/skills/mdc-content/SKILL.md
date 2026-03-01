---
name: mdc-content
description: MDC (Markdown Components) syntax and component usage for writing rich markdown content in Nuxt Content
license: Private
---

# MDC Content

## When to Use

Writing or editing markdown files that use MDC components:

- Creating content in `content/posts/`, `content/docs/`, or any content directory
- Adding interactive components (callouts, accordions, cards, timelines) to markdown
- Using custom Stratafy components in markdown (BlogQuote, HubBanner, ProjectTimeline)
- Writing documents rendered via `<MDC :value="">` (strategy documents, executive summaries)
- Troubleshooting MDC rendering issues
- Building new content components for `app/components/content/`

## Available Guidance

**Progressive loading** - Read what matches your task:

- **[mdc-syntax.md](references/mdc-syntax.md)** - Complete MDC syntax reference (block/inline components, props, slots, data binding)
- **[component-catalog.md](references/component-catalog.md)** - All available MDC components with props, examples, and usage patterns
- **[content-patterns.md](references/content-patterns.md)** - Common content patterns, anti-patterns, and composition recipes

## Quick Reference

### Component Types

| Type   | Syntax                 | Use Case                  |
| ------ | ---------------------- | ------------------------- |
| Block  | `::component` ... `::` | Multi-line content, slots |
| Inline | `:component{props}`    | Single-line, no slots     |

### Prose Components (Primary — use these)

These are Nuxt UI prose components. They work everywhere: blog posts, documents, any MDC context.

| Component     | MDC Name           | Purpose                           |
| ------------- | ------------------ | --------------------------------- |
| Tip           | `::tip`            | Tips, best practices (green)      |
| Note          | `::note`           | Informational notes (blue)        |
| Warning       | `::warning`        | Warnings, cautions (amber)        |
| Caution       | `::caution`        | Critical/dangerous warnings (red) |
| Card          | `::card`           | Content cards with title/icon     |
| CardGroup     | `::card-group`     | Grid layout for multiple cards    |
| Accordion     | `::accordion`      | Expandable sections (with items)  |
| AccordionItem | `::accordion-item` | Individual accordion item         |
| Tabs          | `::tabs`           | Tabbed content panels             |
| TabsItem      | `::tabs-item`      | Individual tab panel              |
| Steps         | `::steps`          | Numbered tutorial steps           |
| Collapsible   | `::collapsible`    | Show/hide detail sections         |
| Badge         | `:badge`           | Inline status labels              |
| Icon          | `:icon`            | Inline icons in prose             |
| Kbd           | `:kbd`             | Keyboard shortcuts                |
| Field         | `::field`          | API/prop documentation            |
| FieldGroup    | `::field-group`    | Group fields together             |
| CodeGroup     | `::code-group`     | Group code blocks with tabs       |

### Custom Stratafy Components

| Component        | MDC Name              | Purpose                             |
| ---------------- | --------------------- | ----------------------------------- |
| BlogQuote        | `::blog-quote`        | Author quotes with avatar           |
| HubBanner        | `::hub-banner`        | Series/hub identification           |
| ProjectTimeline  | `::project-timeline`  | Phased timeline with status         |
| MetricCard       | `::metric-card`       | Dashboard metric with trend         |
| PresentationDeck | `::presentation-deck` | Full-viewport slide container       |
| PresentationPage | `::presentation-page` | Individual slide within a deck      |
| SpeakerNotes     | `::speaker-notes`     | Hidden talking points for presenter |

### Props Syntax

```mdc
<!-- String props -->
::component{title="Hello" color="primary"}

<!-- JSON props (use : prefix) -->
::component{:items='["a", "b", "c"]'}

<!-- YAML props (for complex data) -->
::component
---
items:
  - label: Item 1
    content: Description
---
::
```

### Slots Syntax

```mdc
::card{title="Feature" icon="i-lucide-star"}
Default slot content (markdown supported)

#description
Named slot content
::
```

## Rules

1. **Use prose component names** — `::card` not `::u-card`, `::tip` not `::alert`
2. **Always use kebab-case** for component names in MDC (`BlogQuote` -> `blog-quote`)
3. **Block components need closing** `::` on its own line
4. **Inline components** use single `:` prefix and cannot have slot content
5. **Inline props must be on a single line** — multi-line `{}` does not parse. For complex props, use YAML `---` blocks instead
6. **String values use double quotes** in props: `title="Hello"`
7. **JSON values use single quotes** with `:` prefix: `:items='[...]'`
8. **Slots use `#` prefix** on their own line
9. **Components must exist** in `app/components/content/` (for Nuxt Content) or be registered in `app/plugins/mdc-components.ts` (for `<MDC>` in documents) or be prose components from Nuxt UI
10. **Mermaid diagrams are not supported** — no mermaid dependency installed
11. **Accordion/Tabs use slot-based items** — nest `::accordion-item` and `::tabs-item` children, not YAML `items` prop
12. **Never nest components inside tabs or cards** — putting `::warning`/`::tip`/`::note` inside `::tabs-item` or `::card` produces broken rendering with stray `::` delimiters. Use flat structure with `### ` headings instead of tabs when content needs callouts
13. **Use tabs sparingly** — only for simple text comparisons (e.g., Traditional vs AI-Native). For sections with tables, callouts, or mixed content, use `### ` subheadings instead
