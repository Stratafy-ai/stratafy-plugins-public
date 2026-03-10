# Component Catalog

All components available for use in Stratafy workspace documents.

## Callouts — tip, note, warning, caution

Semantic callout boxes for highlighting information. Each variant has a fixed color and icon.

| Variant     | Color | Use Case                              |
| ----------- | ----- | ------------------------------------- |
| `::tip`     | green | Recommendations, best practices       |
| `::note`    | blue  | Background context, additional info   |
| `::warning` | amber | Risks, things to watch out for        |
| `::caution` | red   | Critical issues, irreversible actions |

**Props:**

| Prop   | Type   | Description                        |
| ------ | ------ | ---------------------------------- |
| `to`   | string | Makes the callout a clickable link |
| `icon` | string | Override the default icon          |

**Examples:**

```mdc
::tip
Focus on validating your highest-impact assumptions first — they're the ones that could invalidate your entire strategy.
::

::note
This strategy was last reviewed in Q4 2025. A quarterly review is recommended.
::

::warning
Three OKRs are off track. Consider a reprioritisation session before the quarter ends.
::

::caution
Changing the corporate strategy will cascade to all functional strategies and linked initiatives.
::
```

---

## card

Content card with optional title, icon, and link. Useful for highlighting key findings, recommendations, or next steps.

**Props:**

| Prop    | Type   | Description                     |
| ------- | ------ | ------------------------------- |
| `title` | string | Card heading                    |
| `icon`  | string | Icon displayed in the card      |
| `color` | string | Card color theme                |
| `to`    | string | Makes the card a clickable link |

**Examples:**

```mdc
::card{title="Key Finding" icon="i-lucide-bar-chart"}
Organizations lose $99M for every $1B invested due to poor strategy execution.
::

::card{title="Recommendation" icon="i-lucide-lightbulb"}
Run a competitive radar scan before Q2 planning to validate market assumptions.
::
```

---

## card-group

Responsive grid layout for multiple cards. Great for executive summaries and strategic pillars.

```mdc
::card-group
  ::card{title="Growth" icon="i-lucide-trending-up"}
  Expand into mid-market segment with targeted GTM motion.
  ::

  ::card{title="Product" icon="i-lucide-box"}
  Ship AI-forward features that differentiate from competitors.
  ::

  ::card{title="People" icon="i-lucide-users"}
  Build leadership bench and scale engineering team to 25.
  ::
::
```

---

## accordion + accordion-item

Expandable content sections. Ideal for FAQ sections, detailed analysis, or supporting evidence that shouldn't clutter the main document.

**accordion-item Props:**

| Prop    | Type   | Required | Description   |
| ------- | ------ | -------- | ------------- |
| `label` | string | Yes      | Section title |
| `icon`  | string | No       | Section icon  |

**Uses nested items (NOT YAML `items` prop).**

```mdc
::accordion
  ::accordion-item{label="What assumptions underpin this strategy?"}
  1. The mid-market segment is underserved by current competitors
  2. Our product can achieve feature parity within 6 months
  3. Customer acquisition cost will decrease as brand awareness grows
  ::

  ::accordion-item{label="What are the key risks?"}
  - Competitor X may launch a similar product in Q2
  - Key engineering hire may not close by deadline
  - Regulatory changes in the EU could affect our pricing model
  ::

  ::accordion-item{label="What decisions are still pending?"}
  - Pricing model for enterprise tier (decision due: March 15)
  - Build vs. buy for analytics engine (decision due: April 1)
  ::
::
```

---

## tabs + tabs-item

Tabbed content panels for comparisons or alternative views.

**tabs-item Props:**

| Prop    | Type   | Required | Description |
| ------- | ------ | -------- | ----------- |
| `label` | string | Yes      | Tab label   |
| `icon`  | string | No       | Tab icon    |

**Use sparingly — only for simple text comparisons.** If tab content needs tables, callouts, or other components, use `### ` headings instead.

```mdc
::tabs
  ::tabs-item{label="Current State" icon="i-lucide-clock"}
  Revenue concentrated in a single segment. Limited brand awareness outside core market. Team capacity stretched across too many initiatives.
  ::

  ::tabs-item{label="Target State" icon="i-lucide-target"}
  Diversified revenue across three segments. Recognised brand in mid-market. Focused team executing on three strategic pillars with clear ownership.
  ::
::
```

---

## steps

Numbered step-by-step sections. Uses `### ` headings as step markers. Ideal for implementation plans, onboarding guides, or process documentation.

```mdc
::steps

### Define Strategic Intent
Articulate the overarching direction — what you will do AND what you won't do. This constrains all downstream decisions.

### Align the Team
Ensure every team member understands how their work connects to strategy. Misalignment here compounds over time.

### Set Measurable Objectives
Create OKRs that test whether your strategy is working. If you can't measure it, it's a hope, not a strategy.

### Monitor & Adapt
Use continuous alignment to detect drift early. Review signals, validate assumptions, and course-correct quarterly.

::
```

---

## collapsible

Show/hide content sections. Good for supporting detail that some readers need and others don't.

**Props:**

| Prop   | Type   | Description                          |
| ------ | ------ | ------------------------------------ |
| `name` | string | Label for what is being shown/hidden |

```mdc
::collapsible{name="methodology details"}
This analysis uses a modified SWOT framework combined with assumption mapping. Each strategic pillar was evaluated against three criteria: market evidence, internal capability, and resource feasibility.
::
```

---

## metric-card

Dashboard-style metric card with animated numbers, optional progress bar, and trend badge. Ideal for executive summaries, OKR scorecards, and strategy reviews.

**Props:**

| Prop           | Type          | Required | Description                                      |
| -------------- | ------------- | -------- | ------------------------------------------------ |
| `title`        | string        | Yes      | Metric name                                      |
| `value`        | number/string | Yes      | Current value                                    |
| `target`       | number/string | No       | Target value (shows progress bar if set)         |
| `unit`         | string        | No       | Unit label (e.g., "%")                           |
| `value-prefix` | string        | No       | Prefix (e.g., "$", "R")                          |
| `value-suffix` | string        | No       | Suffix                                           |
| `trend`        | string        | No       | Trend text (e.g., "+5.2%")                       |
| `icon`         | string        | No       | Lucide icon name                                 |
| `status`       | string        | No       | "on-target", "behind", "ahead" — colors the card |

**Progress bar auto-colors:** >= 90% green, >= 50% amber, < 50% red.

```mdc
::metric-card{title="MRR" value="25000" target="100000" value-prefix="R" trend="+5.2%" icon="i-lucide-dollar-sign" status="behind"}
::

::metric-card{title="Active Coaches" value="12" target="50" trend="+3" icon="i-lucide-users" status="behind"}
::

::metric-card{title="NPS Score" value="72" unit="%" trend="+8" icon="i-lucide-heart" status="on-target"}
::
```

---

## project-timeline

Interactive timeline with status-based styling. Use for initiative roadmaps, quarterly plans, or implementation phases.

**Props:**

| Prop          | Type   | Default      | Description                    |
| ------------- | ------ | ------------ | ------------------------------ |
| `title`       | string | `''`         | Timeline heading               |
| `orientation` | string | `'vertical'` | `'vertical'` or `'horizontal'` |
| `items`       | array  | `[]`         | Timeline items (use YAML)      |

**Status styling:**

| Status        | Color   |
| ------------- | ------- |
| `completed`   | green   |
| `in_progress` | blue    |
| `pending`     | neutral |
| `cancelled`   | red     |
| `delayed`     | orange  |

```mdc
::project-timeline
---
title: GTM Strategy Rollout
items:
  - date: Q1 2026
    title: Foundation
    status: completed
    description: Market research, positioning, initial team hire
  - date: Q2 2026
    title: Launch
    status: in_progress
    description: Product launch, first 10 customers, feedback loops
  - date: Q3 2026
    title: Scale
    status: pending
    description: Channel partnerships, content engine, sales playbook
  - date: Q4 2026
    title: Optimise
    status: pending
    description: Unit economics validation, process automation
---
::
```

> **Important:** Use YAML `---` props for timeline items. Inline JSON in `{}` is fragile with nested quotes.

---

## presentation-deck

Full-viewport slide presentation container. Starts as a meta card in the document; click "Present" to enter fullscreen mode with keyboard navigation.

**Props:**

| Prop          | Type   | Description                        |
| ------------- | ------ | ---------------------------------- |
| `title`       | string | Presentation title (shown in meta) |
| `description` | string | Description text (shown in meta)   |
| `author`      | string | Author name (shown in meta)        |
| `date`        | string | Date string (shown in meta)        |

**Keyboard controls (in presentation mode):**

| Key                    | Action          |
| ---------------------- | --------------- |
| Arrow Down/Right/Space | Next slide      |
| Arrow Up/Left          | Previous slide  |
| Home                   | First slide     |
| End                    | Last slide      |
| Escape                 | Exit fullscreen |

**Features:**

- Slide counter in nav overlay
- Speaker notes popup window (syncs automatically)
- Scroll-snap navigation between slides

```mdc
::::::presentation-deck{title="Q2 Strategy Review" description="Board briefing" author="Sarah Chen" date="March 2026"}

::::presentation-page
# Q2 Strategy Review
## Board Briefing — March 2026

Sarah Chen, CEO
::::

::::presentation-page
# Strategic Priorities

- **Growth:** Expand mid-market segment
- **Product:** Ship AI-forward features
- **People:** Build leadership bench

::speaker-notes
Emphasise that these three pillars haven't changed since Q1 — consistency is the message.
::
::::

::::presentation-page
# Key Metrics

::metric-card{title="ARR" value="1200000" target="2000000" value-prefix="$" trend="+18%" status="behind"}
::

::metric-card{title="Customer Retention" value="94" unit="%" trend="+2%" status="on-target"}
::

::speaker-notes
ARR is behind target but growth rate is accelerating. Retention is strong.
::
::::

::::::
```

**Nesting rules:** The deck must use MORE colons than its children. Use `::::::` (6 colons) for the deck, `::::` (4 colons) for pages, and `::` (2 colons) for components inside pages. **Critical: the closing colons MUST exactly match the opening colon count.** A `::::` opener requires a `::::` closer — using `:::` or `:::::` will fail silently and render literal colons as text.

---

## presentation-page

Individual slide within a `presentation-deck`. Full viewport height with centered content and scaled-up typography.

**No props.** Uses slot content. All markdown and components work inside slides.

---

## speaker-notes

Hidden talking points for presenter view. Content is invisible during presentation but available via the speaker notes popup window.

**No props.** Uses slot content. Full markdown rendering — **bold**, _italic_, lists, and paragraphs all work.

```mdc
::speaker-notes
**Key point:** Revenue model is the unlock.

Talking points:
- Demos overperforming at 200%
- Pipeline full — 10 qualified opportunities
- Need to convert pipeline to paying customers this quarter
::
```

> **Important:** Place `::speaker-notes` inside `::::presentation-page` (4 colons on the page). The deck wrapper must use `::::::` (6 colons) so that `::` closers for inner components don't prematurely close the deck. Always close pages with exactly `::::` (4 colons) — not 3 or 5.

---

## badge

Inline status label.

```mdc
Strategy status: :badge[Active]

Risk level: :badge[High]

Initiative: :badge[In Progress]
```

---

## icon

Inline icon display.

```mdc
:icon{name="i-lucide-check" class="size-5 text-green-500"} Assumption validated

:icon{name="i-lucide-alert-triangle" class="size-5 text-amber-500"} Risk escalated
```
