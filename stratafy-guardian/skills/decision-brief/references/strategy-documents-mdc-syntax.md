# MDC Syntax Reference

Complete reference for MDC (Markdown Components) syntax used in Stratafy documents.

## Block Components

Block components use `::` and can contain markdown content.

```mdc
::component-name
Default slot content here. **Markdown** is supported.
::
```

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
::component{:items='["Item 1", "Item 2", "Item 3"]'}
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

### Boolean Props

```mdc
::field{name="revenue" required}
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

> **Warning: Avoid nesting beyond one level.** Nesting callouts (`::tip`, `::warning`) inside `::tabs-item` or `::card` produces broken rendering. The parser handles simple parent-child nesting (e.g., `::card-group` with `::card` children) but breaks with deeper or mixed nesting.
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

Apply CSS classes and styles to inline elements:

```mdc
Hello [World]{style="color: green;" .custom-class}!

![Image](/img.png){.rounded-lg .shadow-md width="400"}

**bold text**{.text-lg}
```

## Tables

Standard markdown tables work:

```mdc
| Column A | Column B |
| -------- | -------- |
| Value 1  | Value 2  |
```

## Code Blocks

### Standard

````mdc
```typescript
const x = 1
```
````

### Code Group (Tabbed Code Blocks)

````mdc
::code-group

```typescript [config.ts]
export default defineConfig({})
```

```json [data.json]
{ "name": "my-app" }
```

::
````

## Markdown Formatting

Standard markdown is automatically styled when rendered. No special syntax needed:

| Markdown      | Result              |
| ------------- | ------------------- |
| `# Heading`   | Styled headings     |
| `**bold**`    | Bold text           |
| `*italic*`    | Italic text         |
| `[text](url)` | Styled links        |
| `> quote`     | Blockquote          |
| Code blocks   | Syntax highlighting |
| Tables        | Styled tables       |
