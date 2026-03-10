---
name: strategy-documents
description: Create, organise, and format strategy documents and presentations in Stratafy workspaces
license: Apache-2.0
---

# MDC Content — Rich Strategy Documents

## When to Use

Writing or editing strategy documents and presentations in Stratafy:

- Creating strategy documents, executive summaries, or decision briefs
- Building slide presentations for board meetings or coaching sessions
- Adding callouts, metrics, timelines, and structured content to documents
- Formatting workspace documents with cards, tabs, accordions, and other components
- Writing any document that will be rendered in a Stratafy workspace

## Available Guidance

**Progressive loading** — read what matches your task:

- **[mdc-syntax.md](references/mdc-syntax.md)** — Complete MDC syntax reference (block/inline components, props, slots)
- **[component-catalog.md](references/component-catalog.md)** — All available components with props, examples, and usage patterns
- **[content-patterns.md](references/content-patterns.md)** — Document patterns for strategy work: briefs, reviews, presentations

## Quick Reference

### Component Types

| Type   | Syntax                 | Use Case                  |
| ------ | ---------------------- | ------------------------- |
| Block  | `::component` ... `::` | Multi-line content, slots |
| Inline | `:component{props}`    | Single-line, no slots     |

### Callouts

| MDC Name    | Purpose                           |
| ----------- | --------------------------------- |
| `::tip`     | Tips, best practices (green)      |
| `::note`    | Informational notes (blue)        |
| `::warning` | Warnings, cautions (amber)        |
| `::caution` | Critical/dangerous warnings (red) |

### Layout & Structure

| MDC Name           | Purpose                        |
| ------------------ | ------------------------------ |
| `::card`           | Content cards with title/icon  |
| `::card-group`     | Grid layout for multiple cards |
| `::accordion`      | Expandable FAQ/detail sections |
| `::accordion-item` | Individual accordion item      |
| `::tabs`           | Tabbed content panels          |
| `::tabs-item`      | Individual tab panel           |
| `::steps`          | Numbered step-by-step guides   |
| `::collapsible`    | Show/hide detail sections      |

### Strategy Components

| MDC Name                  | Purpose                                   |
| ------------------------- | ----------------------------------------- |
| `::metric-card`           | Dashboard metric with trend               |
| `::project-timeline`      | Phased timeline with status               |
| `::::::presentation-deck` | Full-viewport slide container (6 colons)  |
| `::::presentation-page`   | Individual slide within a deck (4 colons) |
| `::speaker-notes`         | Hidden talking points for presenter       |

### Inline Components

| MDC Name | Purpose               |
| -------- | --------------------- |
| `:badge` | Inline status labels  |
| `:icon`  | Inline icons in prose |

### Props Syntax

```mdc
<!-- String props -->
::component{title="Hello" color="primary"}

<!-- YAML props (for complex data like arrays) -->
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

1. **Always use kebab-case** for component names (`metric-card` not `MetricCard`)
2. **Block components need closing** `::` on its own line
3. **Inline components** use single `:` prefix and cannot have slot content
4. **All props must be on a single line** inside `{}` — for complex data, use YAML `---` blocks instead
5. **String values use double quotes** in props: `title="Hello"`
6. **Accordion/Tabs use nested items** — use `::accordion-item` and `::tabs-item` children, not YAML `items` prop
7. **Never nest components inside tabs or cards** — callouts inside `::tabs-item` or `::card` produce broken rendering. Use `### ` headings instead
8. **Use tabs sparingly** — only for simple text comparisons. For sections with tables or callouts, use headings
9. **Add blank lines** around block components for reliable parsing
10. **Outer containers need more colons** — in MDC, a closer matches the nearest same-count opener. For presentations: deck uses `::::::` (6), pages use `::::` (4), inner components use `::` (2)
11. **Closers MUST match opener colon count** — if you open with `::::` (4 colons), you MUST close with `::::` (4 colons). Using `:::` or `:::::` to close a `::::` opener will fail silently and render literal colons
