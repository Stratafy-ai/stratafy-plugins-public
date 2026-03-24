---
name: signal-intelligence
description: Operate Stratafy's strategic intelligence pipeline for detecting, classifying, and routing signals that could affect strategy
---

# Signal Intelligence

You help users operate Stratafy's strategic intelligence pipeline — detecting, classifying, researching, assessing, and routing signals that could affect strategy.

## What Signals Are

A signal is an early indicator of change in the strategic environment. Signals are raw — they might be a competitor announcement, a market trend, a regulatory change, a customer behaviour shift, or an internal observation. Not every signal matters. The intelligence pipeline separates noise from actionable intelligence.

## The Signal Pipeline

```
Detect → Classify → Research → Assess Impact → Route → Act
```

### 1. Detect — Create Signals
Use `create_signal` when you encounter something that might affect strategy. Include:
- **name** — Clear, descriptive title
- **description** — What was observed, where, when
- **signal_type** — market, competitive, regulatory, technology, customer, internal, economic, social
- **source** — Where this information came from
- **urgency** — low, medium, high, critical

Don't filter too aggressively at this stage. It's better to capture a signal that turns out to be noise than to miss one that matters.

### 2. Classify
Use `classify_signal` to categorise the signal's nature and relevance. Classification helps with routing — a competitive signal goes to different stakeholders than a regulatory one.

### 3. Research
Use `add_signal_research` to attach research findings to a signal. This might include:
- Market data or reports
- Competitor analysis
- Customer feedback
- Expert opinions
- Historical precedent

### 4. Assess Impact
Use `assess_signal_impact` to evaluate how this signal could affect the organisation's strategy. Consider:
- Which strategies are affected?
- How severe is the potential impact?
- How certain are we about the signal's validity?
- What's the time horizon — is this imminent or emerging?

### 5. Route
Use `route_signal` to direct the signal to the right people or processes. Routing determines who needs to know and what action to take.

Use `link_signal_to_strategy` to connect a signal to the strategies it affects. This creates visibility — when reviewing a strategy, all linked signals appear.

### 6. Act
Signals lead to one of several outcomes:
- **Create a risk** — `create_risk` if the signal threatens strategy
- **Create an insight** — `create_insight` if the signal reveals something valuable
- **Create a decision** — `create_decision` if the signal requires a strategic choice
- **Update a strategy** — `update_strategy` if the signal changes the landscape
- **Monitor** — keep watching, no action yet

## Radar Operations

The radar system is Stratafy's structured scanning capability.

### Radar Sessions
Use `create_radar_session` to initiate a scanning exercise. A radar session is a structured effort to scan the environment — like a quarterly competitive review or an annual market assessment.

### Radar Scans
Use `create_radar_scan` within a session to perform specific scans. Each scan focuses on a particular area or question.

### Radar Findings
Use `add_radar_finding` to log what the scan discovered.

Findings lead to:
- **Implications** — Use `add_radar_implication` to document what the finding means for the organisation
- **Recommendations** — Use `add_radar_recommendation` to suggest actions based on the finding

The flow: Scan → Findings → Implications → Recommendations → Signals/Risks/Decisions

## Querying Signals

- `list_signals` — See all signals with optional filtering
- `get_signals_for_strategy` — What signals affect a specific strategy?
- `get_signals_for_entity` — What signals are linked to any entity?
- `get_signal_routes` — Where has a signal been routed?

## Intelligence Patterns for Coaches

### Client Environment Scan
Before a coaching session, scan the client's signal pipeline:
1. `list_signals` — any new unprocessed signals?
2. `get_signals_for_strategy` on key strategies — anything new?
3. `list_radar_scans` — when was the last scan? Is it time for another?

### Competitive Intelligence Workshop
Guide a client through a structured competitive scan:
1. Create a radar session for the exercise
2. Run scans on each major competitor
3. Log findings with implications
4. Convert key findings to signals
5. Assess impact on existing strategies
6. Route signals to relevant strategy owners

### Market Shift Detection
When a client mentions market changes:
1. Create a signal capturing what they observed
2. Research the signal with additional data
3. Assess impact across their strategy tree
4. Create risks or update strategies as needed

## When to Use This Knowledge

Activate this skill when:
- A user mentions competitors, market changes, or trends
- Someone wants to run a competitive or market scan
- The conversation involves radar operations
- A user asks about signals or intelligence
- Environmental scanning or strategic sensing is discussed
