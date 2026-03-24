---
name: radar-scan
description: Run a strategic intelligence scan for competitive analysis, market trends, and environmental factors
---

# /stratafy:radar-scan

Run a strategic intelligence scan — competitive analysis, market trends, regulatory changes, or any environmental factor that could affect strategy. Creates structured findings with implications and recommendations.

## When to Use

- Quarterly competitive intelligence review
- When a client mentions a market shift or competitor move
- Before strategic planning to understand the landscape
- When signals suggest the environment is changing
- Annual market assessment

## Input

Provide one of:
- **A focus area** — "scan competitors in the mid-market SaaS space"
- **A specific competitor** — "scan what Competitor X has been doing"
- **A market question** — "scan for regulatory changes affecting fintech"
- **A broad scan** — "what's happening in our strategic environment?"

## Process

### Step 1: Create Radar Session
I'll use `create_radar_session` to initiate a structured scanning exercise. This creates a container for all findings.

### Step 2: Run Scans
I'll use `create_radar_scan` for each focus area. Scans might cover:
- **Competitive landscape** — What are competitors doing? New products, pricing changes, market moves?
- **Market trends** — What's shifting in the market? New segments, changing buyer behaviour?
- **Technology changes** — What new technologies could disrupt or enable strategy?
- **Regulatory environment** — What regulations are coming? What's changing?
- **Customer signals** — What are customers saying? What unmet needs are emerging?

I'll use web search to gather current intelligence and cross-reference with existing workspace data.

### Step 3: Log Findings
For each significant discovery, I'll use `add_radar_finding` to record:
- What was found
- Source and reliability
- Relevance to the organisation's strategy

### Step 4: Analyse Implications
For each finding, I'll use `add_radar_implication` to document:
- What this means for the organisation
- Which strategies are affected
- How urgent is the response

### Step 5: Generate Recommendations
For significant implications, I'll use `add_radar_recommendation` to suggest:
- Specific actions to take
- Strategic adjustments to consider
- Risks to create or update
- Assumptions to validate or invalidate

### Step 6: Create Signals
Key findings become signals in the workspace using `create_signal`, linked to relevant strategies via `link_signal_to_strategy`. This ensures they're visible in ongoing strategy reviews.

### Step 7: Radar Report
I'll synthesise everything into a structured intelligence briefing:

**Scan Summary**
- What was scanned and why
- Key findings overview

**Findings Detail**
For each significant finding:
- What was discovered
- Implication for strategy
- Recommended response
- Urgency and confidence level

**Strategic Impact Assessment**
- Which strategies are most affected?
- Do any assumptions need revisiting?
- Are there new risks to log?
- Are there opportunities to capture?

**Action Items**
- Immediate actions (this week)
- Near-term actions (this month)
- Strategic considerations (this quarter)

## Scan Templates

### Competitive Scan
Focus: specific competitors or competitive landscape
Output: competitor positioning, product moves, strategic shifts, threat assessment

### Market Scan
Focus: market dynamics, customer behaviour, industry trends
Output: market size/growth, segment shifts, emerging needs, opportunity areas

### Technology Scan
Focus: technology developments relevant to strategy
Output: new capabilities, disruption risks, adoption trends, build/buy/partner decisions

### Regulatory Scan
Focus: regulatory and compliance environment
Output: upcoming regulations, compliance requirements, risk areas, timeline

## For Coaches
- Run a radar scan before a client's quarterly planning session to bring fresh intelligence
- Use findings to challenge client assumptions — "Your strategy assumes X, but the scan shows Y"
- Create signals that the client can track between sessions
- Reference radar findings in client progress reports to demonstrate ongoing value
