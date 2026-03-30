---
description: Design a strategic onboarding plan for a new hire
---

# /stratafy-expert-chro:onboarding-plan

Design a 30/60/90 onboarding plan that connects a new hire to the company's strategy from day one. Not a handbook and Slack tour — a strategic orientation.

## When to Use

- A new hire is starting and you want a structured onboarding plan
- Designing onboarding for a role that's critical to strategy execution
- Improving onboarding to reduce time-to-impact for new team members

## Input

- Role title
- Team
- Start date (or approximate)

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "onboarding-plan"`, `plugin_name: "stratafy-expert-chro"`.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities"]` — company context
- `list_strategies` — which strategies does this role connect to?
- `list_initiatives` — which initiatives will this person contribute to?
- `list_values` — values to embed from day one
- `list_principles` — operating principles to introduce
- `list_teams` — team structure for orientation

### Step 3: Design the Plan

Build a 30/60/90 plan where each phase connects to strategy:

**Days 1-30 (Orient):**
- Understand the mission, vision, and strategy
- Learn the values and principles — not as words, but as behaviours
- Meet key people connected to their strategic area
- Understand which strategies and initiatives their role serves
- Quick win: one small contribution that connects to a real initiative

**Days 31-60 (Contribute):**
- Own a defined piece of work on a strategic initiative
- Understand how their team's work maps to the strategy tree
- Build relationships across teams they'll collaborate with
- Identify one assumption or risk in their domain

**Days 61-90 (Own):**
- Full ownership of their strategic contribution
- Propose improvements based on what they've learned
- First check-in on success criteria from the hiring brief
- Feedback loop: what worked in onboarding, what didn't

### Step 4: Present

```
ONBOARDING PLAN — [Role Title]
Team: [Team Name] | Start: [Date]
Connected strategies: [Strategy 1], [Strategy 2]

━━━ DAYS 1-30: ORIENT ━━━━━━━━━━━━━━━━━
Goal: [What success looks like at day 30]

Week 1:
- [Specific action connected to strategy]
- [Specific action connected to strategy]

Week 2-3:
- [Specific action]
- [Specific action]

Week 4:
- [Quick win deliverable]
- [30-day check-in topics]

━━━ DAYS 31-60: CONTRIBUTE ━━━━━━━━━━━━
Goal: [What success looks like at day 60]

- [Specific ownership area tied to initiative]
- [Specific relationship-building action]
- [Specific strategic contribution]

━━━ DAYS 61-90: OWN ━━━━━━━━━━━━━━━━━━━
Goal: [What success looks like at day 90]

- [Full ownership milestone]
- [Strategic contribution milestone]
- [Feedback and adjustment]

━━━ SUCCESS CRITERIA ━━━━━━━━━━━━━━━━━━━
At 90 days, this person is successful if:
1. [Measurable outcome linked to strategy]
2. [Measurable outcome linked to strategy]
3. [Cultural integration indicator]
```

### Step 5: Offer Follow-ups

- "Want to review the hiring brief for this role?" — `/stratafy-expert-chro:hiring-brief`
- "Want to check how this fits the team?" — `/stratafy-expert-chro:team-health`

## Provenance Context

On every mutation, include:
- `_source_plugin`: "stratafy-expert-chro"
- `_source_command`: "onboarding-plan"
- `_change_reasoning`: Brief explanation of why this change is being made

## Rules

1. **Strategy from day one.** Every onboarding action connects to the company's strategy. No generic "read the handbook" items.
2. **Values are behaviours.** Don't just list values — describe what they look like in practice for this specific role.
3. **Quick wins matter.** A new hire who contributes to a real initiative in week 3 builds confidence and belonging faster than one who reads docs for a month.
4. **Relationships are strategic.** Who this person meets in their first 30 days shapes how they understand the company. Be intentional.
5. **Measurable.** Success criteria are concrete, not "fits in well."
6. When referencing strategies or initiatives, use markdown links with the `urls.detail` URL from tool responses.
