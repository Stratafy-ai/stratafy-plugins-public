# /stratafy:team-rhythm

Dashboard view of team plugin adoption — who's building strategic rhythm habits, who's not, and what the patterns tell you about organisational alignment.

## When to Use

- Monday morning — "is the team using the rhythm?"
- Before a leadership meeting — adoption stats for the week
- Monthly review — tracking habit formation trends
- When onboarding new team members — who needs a nudge?
- Diagnosing engagement gaps — "why isn't the sales team using this?"

## Input

Provide one of:
- **Nothing** — Full dashboard for the last 7 days
- **A timeframe** — "team rhythm for the last 30 days" or "this month"
- **A focus** — "show me daily rhythm adoption" or "who's not using it?"

## Process

### Step 1: Log Usage
Call `log_activity` with `activity_type: "command_usage"`, `description: "team-rhythm"`.

### Step 2: Gather Data

Call `get_team_rhythm` with the appropriate `days` parameter (default 7).

This returns:
- Adoption rates (total, daily rhythm, weekly rhythm, coaching)
- Command breakdown (usage counts, unique users)
- Per-user activity (which commands, how many active days)
- Daily heatmap data (who used what on each day)
- Inactive members (in workspace but no command usage)

### Step 3: Present the Dashboard

```
TEAM RHYTHM — [Workspace Name]
Period: [date range] ([N] days)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 ADOPTION OVERVIEW
  Team members using plugin:    [X]/[Y]  ([Z]%)
  Daily rhythm (lets-go/eod):   [X]/[Y]  ([Z]%)
  Weekly rhythm (start/review): [X]/[Y]  ([Z]%)
  Coaching (coach/advise):      [X]/[Y]  ([Z]%)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 DAILY RHYTHM HEATMAP
              Mon   Tue   Wed   Thu   Fri
[Name]        🟢🟢  🟢🟢  🟢🟢  🟢⚪  ⚪⚪
[Name]        🟢🟢  🟢⚪  🟢🟢  🟢🟢  🟢🟢
[Name]        🟢⚪  ⚪⚪  🟢⚪  ⚪⚪  ⚪⚪
[Name]        ⚪⚪  ⚪⚪  ⚪⚪  ⚪⚪  ⚪⚪

🟢🟢 = start + end  |  🟢⚪ = start only  |  ⚪⚪ = no activity

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔥 COMMAND USAGE
  Command                Total    Users
  /lets-go               [X]      [Y]
  /start-the-week        [X]      [Y]
  /call-it-a-day         [X]      [Y]
  /advise-me             [X]      [Y]
  /coach-me              [X]      [Y]
  /catch-me-up           [X]      [Y]
  /why-are-we-doing-this [X]      [Y]
  /week-in-review        [X]      [Y]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

👤 PER-USER BREAKDOWN
  [Name]  — [X] active days, [Y] total commands
    Most used: /lets-go ([N]x), /advise-me ([N]x)

  [Name]  — [X] active days, [Y] total commands
    Most used: /start-the-week ([N]x), /coach-me ([N]x)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ NOT ENGAGED
  [Name] — workspace member, no plugin usage this period
  [Name] — workspace member, no plugin usage this period

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 OBSERVATIONS & RECOMMENDATIONS

[Synthesise patterns into 3-5 actionable observations]

1. [Observation about adoption pattern] — [recommendation]
2. [Observation about engagement gap] — [recommendation]
3. [Observation about command preferences] — [recommendation]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 4: Synthesise Observations

Based on the data, identify patterns and make recommendations:

**Adoption patterns:**
- If <50% adoption: "Most of the team isn't using the plugin yet. Consider a team session to demo the workflow."
- If high daily but low weekly: "Team uses daily rhythm but skips the weekly briefing. The Monday/Friday habit hasn't formed."
- If high start but low end-of-day: "People start with /lets-go but skip reflection. The reflection is where insights get captured — worth encouraging."

**Engagement gaps:**
- If a whole team/role is missing: "No sales team members are using the plugin. This might be a tool adoption issue or a sign they don't see the strategic connection to their work."
- If specific individuals are inactive: "3 members have never used a command. They may need onboarding or a 1:1 demo."

**Usage patterns:**
- High coaching usage: "4 people used /coach-me or /advise-me this week — the team is actively using AI for thinking partnership, not just task management."
- High /why-are-we-doing-this: "Multiple people are asking 'why' — could signal confusion about priorities or a need for better strategic communication."
- High /catch-me-up: "Several people needed to catch up — consider if the team has enough visibility into strategy changes."

**Trend observations (for 14+ day periods):**
- Is adoption growing week over week?
- Are habits forming (consistent daily usage) or sporadic?
- Which commands are gaining/losing traction?

## Rules

- Present data cleanly — this is a dashboard, not a report. Scannable in 60 seconds.
- The heatmap is the most powerful visual — show it for the last 5 working days.
- "Not engaged" section should be factual, not judgmental. "No usage" not "failing to adopt."
- Observations should be specific and actionable, not generic.
- If the dataset is small (early adoption), say so: "Early days — only 3 days of data. Patterns will emerge over 2-3 weeks."
- For longer periods (30+ days), group by week and show trend lines instead of daily heatmaps.
- Offer follow-up: "Want me to send a nudge to inactive members?" or "Want to see the trend over the last month?"
