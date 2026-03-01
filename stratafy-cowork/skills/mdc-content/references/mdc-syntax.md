# MDC Syntax Reference

Complete reference for Markdown Components (MDC) syntax in Nuxt Content.

## Block Components

Block components use `::` and can contain markdown content or nested components as slots.

```mdc
::component-name
Default slot content here. **Markdown** is supported.
::
```

Components must have a `<slot />` in their template to accept formatted text.

### With Props (Inline)

**All inline props must be on a single line.** Multi-line `{}` does not parse correctly.

```mdc
::card{title="Feature Highlight" icon="i-lucide-star" color="primary"}
This is the default slot content.
::
```

### With Props (YAML)

Better readability for complex data:

```mdc
::project-timeline
---
title: Implementation Roadmap
items:
  - date: Q1 2026
    title: Foundation
    status: completed
  - date: Q2 2026
    title: Growth
    status: in_progress
---
::
```

## Inline Components

Single-line components using `:` prefix. Cannot have slot content.

```mdc
:icon{name="i-lucide-check" class="size-5 text-green-500"}
```

Inline components render within the current paragraph flow.

## Props

### String Values

Use double quotes:

```mdc
::component{title="Hello World" color="primary"}
::
```

### JSON Values (Arrays/Objects)

Use `:` prefix with single quotes:

```mdc
::component{:items='["Nuxt", "Vue", "React"]'}
::
```

### YAML Props Block

For complex data, use a YAML block between `---` delimiters:

```mdc
::component
---
title: My Component
items:
  - label: Item 1
    content: Description
  - label: Item 2
    content: Another description
---
::
```

### Frontmatter Binding

Bind to frontmatter variables with `:` prefix:

```mdc
---
alertType: warning
---

::card{:color="alertType"}
Dynamic color from frontmatter.
::
```

### Boolean Props

```mdc
::field{name="email" required}
::
```

## Slots

### Default Slot

Top-level content inside a block component:

```mdc
::tip
This is the **default slot** content. Full markdown supported.
::
```

### Named Slots

Use `#slot-name` on its own line:

```mdc
::card{title="Feature"}
Default slot body content.

#description
Extra description in a named slot.
::
```

### Slot with mdc-unwrap

In component templates, use `mdc-unwrap="p"` to strip paragraph wrappers:

```vue
<template>
  <h1><slot mdc-unwrap="p" /></h1>
</template>
```

This prevents `<p>` tags from wrapping content inside elements like `<h1>`.

## Nested Components

Components can be nested. Each nesting level adds one `:` to the delimiters:

```mdc
::card-group
  :::card{title="First Card" icon="i-lucide-star"}
  First card content.
  :::

  :::card{title="Second Card" icon="i-lucide-heart"}
  Second card content.
  :::
::
```

> **Warning: Avoid nesting beyond one level.** Nesting callouts (`::tip`, `::warning`, etc.) inside `::tabs-item` or `::card` produces broken rendering — stray `::` delimiters appear in the output. The MDC parser handles simple parent→child nesting (e.g., `::card-group` → `::card`) but breaks with deeper nesting or mixed component types.
>
> **Instead of nesting, flatten with headings:**
>
> ```mdc
> <!-- BAD: callout nested inside tabs-item -->
> ::tabs
>   ::tabs-item{label="Analysis"}
>   ::warning
>   This breaks rendering.
>   ::
>   ::
> ::
>
> <!-- GOOD: flat structure with heading -->
> ### Analysis
>
> ::warning
> This renders correctly.
> ::
> ```

## Inline Attributes

Apply CSS classes, IDs, and styles to inline elements:

```mdc
Hello [World]{style="color: green;" .custom-class #custom-id}!

![Image](/img.png){.rounded-lg .shadow-md width="400"}

`inline code`{.text-primary}

**bold text**{.text-lg}
```

## Data Binding

Reference frontmatter variables in content:

```mdc
---
title: My Article
version: 2.0
---

# {{ $doc.title }}

Current version: {{ $doc.version || 'unknown' }}
```

Variables can also be passed via `ContentRenderer`:

```vue
<ContentRenderer :value="data" :data="{ customVar: 'value' }" />
```

## Frontmatter

YAML block at the top of markdown files:

```mdc
---
title: 'Page Title'
description: 'Meta description'
navigation: true
date: '2026-01-01'
---
```

### Native Parameters

- `title` - Defaults to first `<h1>` if not set
- `description` - Defaults to first `<p>` if not set
- `navigation` - Boolean, include in navigation (default: `true`)

## Code Blocks

### Standard

````mdc
```typescript
const x = 1
```
````

### With Filename

````mdc
```typescript [nuxt.config.ts]
export default defineNuxtConfig({})
```
````

### Code Group (Tabbed Code Blocks)

````mdc
::code-group

```typescript [nuxt.config.ts]
export default defineNuxtConfig({})
```

```json [package.json]
{ "name": "my-app" }
```

::
````

## Excerpts

Use `<!--more-->` to split content into an excerpt (preview) and full body. Everything above the divider becomes the excerpt; everything below is only shown in the full content view.

```mdc
---
title: Introduction
---

Learn how to use continuous alignment to close the strategy execution gap.

<!--more-->

Full article content starts here. This won't appear in blog listing cards or previews.
```

**How it works:**

- The excerpt is available as the `description` property unless `description` is explicitly set in frontmatter
- If no `<!--more-->` divider exists, the excerpt is undefined
- Requires `excerpt` field in your collection schema to be accessible:

```typescript
const content = defineCollection({
  type: 'page',
  source: '**',
  schema: z.object({
    excerpt: z.object({
      type: z.string(),
      children: z.any(),
    }),
  }),
})
```

**When to use:** Blog posts where you want a rich preview (with markdown formatting) in listing pages rather than a plain-text `description` from frontmatter.

## Tables

Standard markdown tables work:

```mdc
| Column A | Column B |
| -------- | -------- |
| Value 1  | Value 2  |
```

## Prose Components

The MDC module replaces standard HTML tags with styled Vue components automatically. These affect how your markdown renders — no special syntax needed from the author.

| Markdown      | Rendered By         | Notes                       |
| ------------- | ------------------- | --------------------------- |
| `# Heading`   | `ProseH1`-`ProseH6` | Auto-generates anchor links |
| `**bold**`    | `ProseStrong`       |                             |
| `*italic*`    | `ProseEm`           |                             |
| `[link](url)` | `ProseA`            |                             |
| `![img](url)` | `ProseImg`          |                             |
| `` `code` ``  | `ProseCode`         | Inline code                 |
| Code blocks   | `ProsePre`          | With syntax highlighting    |
| `> quote`     | `ProseBlockquote`   |                             |
| Tables        | `ProseTable`        | Full table component set    |

Custom prose components can be created in `app/components/content/` (e.g., `ProseBlockquote.vue`) to override default rendering.
