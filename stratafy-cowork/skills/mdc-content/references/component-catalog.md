# MDC Component Catalog

All components available for use in Stratafy markdown content files.

## Prose Components

Nuxt UI v4 ships prose components designed for MDC. These work in all contexts: blog posts (`<ContentRenderer>`), documents (`<MDC :value="">`), and any MDC rendering.

**Use these names (without `u-` prefix) for maximum compatibility.**

### Callouts — tip, note, warning, caution

Semantic callout boxes for highlighting information. Each variant has a fixed color and icon.

| Variant     | MDC Name | Color     | Use Case                    |
| ----------- | -------- | --------- | --------------------------- |
| `::tip`     | Tip      | `success` | Tips, best practices, green |
| `::note`    | Note     | `info`    | Informational notes, blue   |
| `::warning` | Warning  | `warning` | Warnings, cautions, amber   |
| `::caution` | Caution  | `caution` | Critical/dangerous, red     |

**Props (inherited from base Callout):**

| Prop     | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| `to`     | string | Makes the callout a clickable link        |
| `target` | string | Link target (`_blank` for external links) |
| `icon`   | string | Override the default icon                 |
| `color`  | string | Override the default color                |

**Basic callouts:**

```mdc
::tip
Use continuous alignment to keep strategy connected to reality.
::

::note
This feature requires workspace admin permissions.
::

::warning
Changing the strategy model will reset all existing alignment scores.
::

::caution
This action cannot be undone. All data will be permanently deleted.
::
```

**Callout as a link:**

```mdc
::tip
---
to: https://ui.nuxt.com/getting-started/installation
target: _blank
---
Learn more about how to take the most out of Nuxt UI!
::
```

**Mapping from old alert patterns:**

| Old `u-alert` Pattern            | New Prose Pattern |
| -------------------------------- | ----------------- |
| `color="info"` TL;DR             | `::note`          |
| `color="primary"` Insight        | `::tip`           |
| `color="success"` Key Takeaway   | `::tip`           |
| `color="warning"` Common Mistake | `::warning`       |
| `color="error"` Critical Issue   | `::caution`       |

---

### card

Content card with optional title, icon, and link.

**Props:**

| Prop          | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| `title`       | string | Card heading                    |
| `icon`        | string | Icon displayed in the card      |
| `color`       | string | Card color theme                |
| `to`          | string | Makes the card a clickable link |
| `target`      | string | Link target                     |
| `description` | string | Card description text           |

**Basic card:**

```mdc
::card{title="Key Finding" icon="i-lucide-bar-chart"}
Organizations lose $99M for every $1B invested due to poor strategy execution.
::
```

**Card as a link:**

```mdc
::card{title="Continuous Alignment" icon="i-lucide-activity" to="/features/alignment"}
Monitor strategy execution with live dashboards and automated alerts.
::
```

---

### card-group

Responsive grid layout for multiple cards.

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

---

### accordion + accordion-item

Expandable content sections. Uses slot-based items (NOT YAML `items` prop).

**accordion-item Props:**

| Prop          | Type   | Required | Description          |
| ------------- | ------ | -------- | -------------------- |
| `label`       | string | Yes      | Section title        |
| `description` | string | No       | Subtitle/description |
| `icon`        | string | No       | Icon next to label   |

**FAQ example:**

```mdc
::accordion
  ::accordion-item{label="What is strategy execution?"}
  Strategy execution is the process of turning strategic plans into action and results. It bridges the gap between what an organization plans to do and what it actually achieves.
  ::

  ::accordion-item{label="Why do strategies fail?"}
  Most strategies fail due to poor communication, lack of alignment, missing feedback loops, and resource misallocation.
  ::

  ::accordion-item{label="How does continuous alignment help?"}
  Continuous alignment keeps strategy connected to reality through regular check-ins, real-time monitoring, and automated adaptation.
  ::
::
```

---

### tabs + tabs-item

Tabbed content panels for comparisons or alternative views.

**tabs Props:**

| Prop           | Type   | Default | Description          |
| -------------- | ------ | ------- | -------------------- |
| `defaultValue` | string | `"0"`   | Default active tab   |
| `sync`         | string | —       | Sync key across tabs |

**tabs-item Props:**

| Prop          | Type   | Required | Description     |
| ------------- | ------ | -------- | --------------- |
| `label`       | string | Yes      | Tab label       |
| `description` | string | No       | Tab description |
| `icon`        | string | No       | Tab icon        |

**Comparison tabs:**

```mdc
::tabs
  ::tabs-item{label="Traditional" icon="i-lucide-file-text"}
  Annual strategic plans reviewed quarterly. Updates happen in boardrooms. Teams learn about changes weeks later.
  ::

  ::tabs-item{label="AI-Native" icon="i-lucide-activity"}
  Living strategy updated continuously. AI agents monitor execution in real-time. Teams get immediate feedback loops.
  ::
::
```

---

### steps

Numbered step-by-step sections. Uses `### ` headings as step markers.

**Props:**

| Prop    | Type   | Description                       |
| ------- | ------ | --------------------------------- |
| `level` | number | Heading level to use (default: 3) |

```mdc
::steps

### Define Your Strategy
Write a clear strategic intent with measurable objectives.

### Align Your Team
Ensure every team member understands how their work connects to strategy.

### Monitor & Adapt
Use continuous alignment to detect drift and course-correct.

::
```

**Notes:**

- Each `### ` heading becomes a numbered step
- Content between headings becomes the step description
- Markdown formatting works within steps

---

### collapsible

Show/hide content sections.

**Props:**

| Prop        | Type   | Description                           |
| ----------- | ------ | ------------------------------------- |
| `icon`      | string | Custom icon (default: chevron)        |
| `name`      | string | Label for what is being shown/hidden  |
| `openText`  | string | Text when collapsed (default: "Show") |
| `closeText` | string | Text when expanded (default: "Hide")  |

```mdc
::collapsible{name="research methodology"}
The research methodology involved surveys of 500+ organizations across 12 industries, with longitudinal tracking over 18 months.
::
```

---

### badge

Inline status label. Uses slot content (not a `label` prop).

```mdc
This feature is :badge[Beta] and may change.

Status: :badge[Active] — Version :badge[v2.0]
```

---

### icon

Inline icon display.

**Props:**

| Prop   | Type   | Description                            |
| ------ | ------ | -------------------------------------- |
| `name` | string | Icon name (e.g., `i-lucide-lightbulb`) |

```mdc
:icon{name="i-lucide-check" class="size-5 text-green-500"} Task completed
```

---

### kbd

Keyboard key display.

**Props:**

| Prop    | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| `value` | string | Key to display (`meta`, `shift`, `alt`, etc) |

```mdc
Press :kbd{value="meta"} + :kbd{value="K"} to open the command palette.
```

---

### field + field-group

Document API parameters, component props, or configuration options.

**field Props:**

| Prop          | Type    | Description                   |
| ------------- | ------- | ----------------------------- |
| `name`        | string  | Parameter/prop name           |
| `type`        | string  | Type annotation               |
| `description` | string  | Description text              |
| `required`    | boolean | Whether the field is required |

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

---

### code-group

Group multiple code blocks with tabs.

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

---

## Custom Stratafy Components

Located in `app/components/content/`. Auto-registered by Nuxt Content.

### blog-quote

Author attribution quote block with avatar.

**Props:**

| Prop     | Type   | Required | Description              |
| -------- | ------ | -------- | ------------------------ |
| `quote`  | string | Yes      | The quote text           |
| `author` | string | Yes      | Author name              |
| `role`   | string | No       | Author's role/title      |
| `link`   | string | No       | Link to author's profile |
| `avatar` | string | No       | Avatar image URL         |

**Example:**

```mdc
::blog-quote{quote="Speed drives your learning cycle. The faster you iterate, the faster you learn." author="Jaryd Hermann" role="Founder & CEO" link="https://x.com/jaryd" avatar="https://ui-avatars.com/api/?name=Jaryd+Hermann&background=3b82f6&color=fff&size=120"}
::
```

> **Important:** All props must be on a single line inside `{}`. Multi-line `{}` props do not parse correctly.

---

### hub-banner

Series identification banner linking to the hub/series page.

**Props:**

| Prop  | Type   | Required | Description                              |
| ----- | ------ | -------- | ---------------------------------------- |
| `hub` | string | Yes      | Hub slug (must match hub-data constants) |

**Example:**

```mdc
::hub-banner{hub="why-strategies-fail"}
::
```

---

### project-timeline

Interactive timeline with status-based styling.

**Props:**

| Prop          | Type         | Required | Default      | Description                           |
| ------------- | ------------ | -------- | ------------ | ------------------------------------- |
| `title`       | string       | No       | `''`         | Timeline heading                      |
| `items`       | array/string | No       | `[]`         | Timeline items (JSON string or array) |
| `color`       | string       | No       | `'primary'`  | Base color                            |
| `size`        | string       | No       | `'md'`       | Size variant                          |
| `orientation` | string       | No       | `'vertical'` | `'vertical'` or `'horizontal'`        |

**Status Auto-Styling:**

| Status        | Icon                    | Color   |
| ------------- | ----------------------- | ------- |
| `completed`   | `i-lucide-check-circle` | green   |
| `in_progress` | `i-lucide-play-circle`  | blue    |
| `pending`     | `i-lucide-clock`        | neutral |
| `cancelled`   | `i-lucide-x-circle`     | red     |
| `delayed`     | `i-lucide-alert-circle` | orange  |

**Example:**

```mdc
::project-timeline
---
title: Implementation Roadmap
orientation: vertical
items:
  - date: Q1 2026
    title: Foundation
    status: completed
    description: Core platform launch
  - date: Q2 2026
    title: Growth
    status: in_progress
    description: User acquisition phase
  - date: Q3 2026
    title: Scale
    status: pending
    description: Enterprise features
---
::
```

> **Important:** Use YAML `---` props for complex data like arrays/objects. Inline JSON in `{}` is fragile with nested quotes.

---

### metric-card

Dashboard-style metric card with animated numbers, optional progress bar, and trend badge. Ideal for executive summaries and reports.

**Props:**

| Prop          | Type          | Required | Default | Description                                          |
| ------------- | ------------- | -------- | ------- | ---------------------------------------------------- |
| `title`       | string        | Yes      | —       | Metric name (e.g., "MRR")                            |
| `value`       | number/string | Yes      | —       | Current value                                        |
| `target`      | number/string | No       | —       | Target value (shows progress bar if set)             |
| `unit`        | string        | No       | `''`    | Unit label (e.g., "ZAR", "%")                        |
| `valuePrefix` | string        | No       | `''`    | Value prefix (e.g., "R") — use `value-prefix` in MDC |
| `valueSuffix` | string        | No       | `''`    | Value suffix — use `value-suffix` in MDC             |
| `trend`       | string        | No       | `''`    | Trend text (e.g., "+5.2%")                           |
| `icon`        | string        | No       | `''`    | Lucide icon name                                     |
| `status`      | string        | No       | —       | "on-target", "behind", "ahead" — colors the card     |

**Progress bar colors (auto-calculated from value/target):**

| Percentage | Color   |
| ---------- | ------- |
| >= 90%     | green   |
| >= 50%     | warning |
| < 50%      | red     |

**Basic metric:**

```mdc
::metric-card{title="Active Users" value="1234" icon="i-lucide-users"}
::
```

**Metric with target and trend:**

```mdc
::metric-card{title="MRR" value="25000" target="100000" value-prefix="R" trend="+5.2%" icon="i-lucide-dollar-sign" status="behind"}
::
```

**Percentage metric:**

```mdc
::metric-card{title="Conversion Rate" value="3.2" unit="%" trend="+0.5%" status="ahead"}
::
```

> **Important:** All props must be on a single line inside `{}`. Values are passed as strings by MDC and parsed as numbers internally.

---

### presentation-deck

Full-viewport slide presentation container. Teleports to `<body>` as a fixed overlay with scroll-snap navigation, keyboard controls, and a slide counter. Starts in normal view with a meta card; click "Present" to enter fullscreen mode.

**Props:**

| Prop          | Type   | Required | Description                        |
| ------------- | ------ | -------- | ---------------------------------- |
| `title`       | string | No       | Presentation title (shown in meta) |
| `description` | string | No       | Description text (shown in meta)   |
| `author`      | string | No       | Author name (shown in meta)        |
| `date`        | string | No       | Date string (shown in meta)        |

**Keyboard controls (in presentation mode):**

| Key                    | Action          |
| ---------------------- | --------------- |
| Arrow Down/Right/Space | Next slide      |
| Arrow Up/Left          | Previous slide  |
| Home                   | First slide     |
| End                    | Last slide      |
| Escape                 | Exit fullscreen |

**Features:**

- IntersectionObserver tracks current slide
- Fixed nav overlay: close button, speaker notes button (if notes exist), slide counter, next button
- Speaker notes popup window syncs via BroadcastChannel
- `not-prose` class applied to escape parent prose typography

**Basic deck:**

```mdc
::presentation-deck{title="My Presentation" description="Overview deck" author="Name" date="March 2026"}

::::presentation-page
# First Slide
Content here
::::

::::presentation-page
# Second Slide
More content
::::

::
```

**Nesting rules:** Use `::::` (4 colons) for `presentation-page` so inner components (`::card`, `::callout`, `::metric-card`) can nest with 2-3 colons.

---

### presentation-page

Individual slide within a `presentation-deck`. Full viewport height with centered content and scaled-up typography.

**No props.** Uses slot content.

**Styling:**

- `height: 100dvh`, `scroll-snap-align: start`
- Centered flex layout, `max-width: 64rem`, padding `4rem 3rem`
- h1: 3rem, h2: 2.25rem, h3: 1.75rem, body: 1.25rem
- `overflow-y: auto` for long slides

```mdc
::::presentation-page
# Slide Title

- Bullet point one
- Bullet point two

::card{title="Feature" icon="i-lucide-star"}
Card content inside a slide
::

::speaker-notes
Talking points for this slide go here.
::
::::
```

---

### speaker-notes

Hidden talking points for presenter view. Content is invisible during presentation but available via the speaker notes popup window (opened from the nav overlay).

**No props.** Uses slot content. Full markdown rendering — **bold**, _italic_, `code`, lists, and paragraphs all work since MDC renders the content before it's passed to the popup.

```mdc
::speaker-notes
**Key point:** Revenue model is the unlock.

Talking points:
- Demos overperforming at *200%*
- Pipeline full at `10/10`
- Need to convert to **paying customers**
::
```

> **Important:** Place `::speaker-notes` inside `::::presentation-page` (with 4 colons on the page) so the 2-colon notes component nests correctly.

---

## Adding New Components

### For Nuxt Content (blog posts, docs)

1. Create a Vue component in `app/components/content/`
2. Name it in PascalCase (e.g., `FeatureCard.vue`)
3. Include a `<slot />` if it should accept markdown content
4. Use `mdc-unwrap="p"` on slots where you don't want paragraph wrapping
5. The component is automatically available as `::feature-card` in MDC

### For Document `<MDC>` rendering (workspace documents)

1. Create a Vue component in `app/components/` (top-level, not `content/`)
2. Register it in `app/plugins/mdc-components.ts`:
   ```ts
   import MyComponent from '~/components/MyComponent.vue'
   nuxtApp.vueApp.component('MyComponent', MyComponent)
   ```
3. The component is now available as `::my-component` in any document rendered with `<MDC :value="">`

```vue
<script setup lang="ts">
defineProps<{
  title: string
  icon?: string
}>()
</script>

<template>
  <div class="my-4 p-4 rounded-lg border">
    <div class="flex items-center gap-2 mb-2">
      <UIcon v-if="icon" :name="icon" />
      <h3 class="font-semibold">{{ title }}</h3>
    </div>
    <slot />
  </div>
</template>
```

Usage:

```mdc
::feature-card{title="Real-time Alignment" icon="i-lucide-activity"}
Monitor strategy execution with **live dashboards** and automated alerts.
::
```
