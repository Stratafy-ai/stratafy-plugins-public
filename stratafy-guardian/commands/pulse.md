---
name: pulse
description: Get a quick strategic health check of the current workspace
---

# /stratafy:pulse

Get a quick strategic health check of the current workspace. Shows what matters right now — strategic focus, execution health, top risks, and pending decisions.

## When to Use

- Start of your day — "What should I focus on?"
- Before a meeting — quick context on where things stand
- Checking in on a client workspace between sessions
- Any time you want a fast strategic orientation

## Input

Optional:
- **A specific strategy or area** — "pulse on our GTM strategy"
- **A time period** — "pulse since last week"
- **Nothing** — I'll give you the full workspace pulse

## Process

### Step 1: Gather State
I'll pull data from multiple sources simultaneously:
- `get_workspace_snapshot` — overall landscape
- `list_objectives` — OKR health across the board
- `get_high_risk_items` — top risks by score
- `get_pending_decisions` — what's waiting for a decision
- `list_assumptions` — any recently invalidated assumptions
- `list_signals` — unprocessed signals

### Step 2: Analyse & Summarise
I'll synthesise this into a brief covering:

**Strategic Focus**
- What are the active strategies and their current emphasis?
- Any strategies that changed status recently?

**Execution Health**
- OKR progress — how many on track, at risk, off track?
- Initiative status — anything stuck or overdue?
- Key metrics trending up or down?

**Risk & Assumption Watch**
- Top 3 risks by severity
- Any assumptions recently validated or invalidated
- New risks since last check

**Decisions Needed**
- Pending decisions awaiting input
- How long they've been pending

**Signal Queue**
- Unprocessed signals that need attention
- Any high-urgency signals

### Step 3: Recommendations
Based on the pulse, I'll suggest 2-3 specific actions:
- "Risk X has escalated — consider scheduling a mitigation session"
- "3 OKRs are off track — a reprioritisation conversation may be needed"
- "Signal about competitor Y needs processing — should we run a radar scan?"

## Output Format

```
STRATEGIC PULSE — [Workspace Name]
Date: [today]

OVERALL HEALTH: [Green/Amber/Red]

FOCUS: [1-2 sentence summary of current strategic direction]

EXECUTION:
  OKRs: X on track | Y at risk | Z off track
  Initiatives: X active | Y overdue

TOP RISKS:
  1. [Risk name] — [likelihood] × [impact]
  2. [Risk name] — [likelihood] × [impact]

DECISIONS PENDING: X items (oldest: Y days)

SIGNALS: X unprocessed

RECOMMENDED ACTIONS:
  1. [Action]
  2. [Action]
  3. [Action]
```

## For Coaches
When running pulse across multiple clients:
- Switch to each client workspace and run pulse
- I'll maintain context so you can compare across clients
- Ask me "pulse all clients" and I'll guide you through each workspace
