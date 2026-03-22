# /stratafy-team:lets-go

Start your day — pull tasks, connect them to strategy, and get into focused execution.

## Process

### Step 1: Get User Context
Call `get_user_context` with `command_name: "lets-go"`, `plugin_name: "stratafy-team"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Gather Context

In parallel:
- `get_workspace_snapshot` with `sections: ["foundation", "strategies", "key_priorities", "metrics"]` and `_source_plugin: "stratafy-team"` — Workspace context filtered to your role
- `list_metrics` — Key metrics with current status (for quick health pulse)

**Do NOT call `list_strategies` or `list_key_priorities` separately** — the snapshot includes both and avoids duplicate large payloads.

### Step 3: Pull Tasks from Linear

Use `list_issues` with assignee set to the user's email to find all issues assigned to them. Focus on:
1. **In-progress** issues first — what's already started
2. **Todo/upcoming** issues — what's next
3. **Blocked** issues — surface these early

If Linear tools aren't available, ask the user what they're working on today.

### Step 4: Connect Tasks to Strategy

For each Linear issue:
- Check its `project.id` — if it matches an initiative's `[linear:PROJECT_ID]` tag in the strategy tree, that task is automatically linked
- For tasks without a matching project ID, use name/description matching against the strategy tree
- Flag unmapped tasks — not as a problem, just as an observation

### Step 5: Present the Plan

Adapt the presentation to the user's role (see role-adaptation skill).

```
Good [morning/afternoon]! Here's your setup for today.

🎯 COMPANY FOCUS
[Key priorities — 2-3 bullets max]

📊 QUICK PULSE
[2-3 key metrics with status: 🟢🟡🔴]

📋 YOUR TASKS (X items)

IN PROGRESS:
  1. [Task name] → [Initiative] → [Strategy]
     Status: [current status]
     [One line on why this matters strategically]

  2. [Task name] → ⚠️ Unmapped
     [Note: couldn't connect this to a strategy]

UP NEXT:
  3. [Task name] → [Initiative] → [Strategy]
  4. [Task name] → [Initiative] → [Strategy]

BLOCKED:
  5. [Task name] — Blocked by: [reason]
     💡 Unblocking idea: [suggestion]

🏁 SUGGESTED FOCUS ORDER
1. [Task] — [why first: urgency/strategic importance/quick win]
2. [Task] — [why second]
3. [Task] — [why third]

🤝 HOW I CAN HELP
- [Task 1]: I can [draft/research/analyse/review] the [specific thing]
- [Task 3]: I can help unblock by [specific suggestion]
```

### Step 6: Execute Together

Once the user picks a task, dive in and help them do the work. Don't just plan — execute.

## Rules

- Be energising, not overwhelming. This is the start of a work session.
- Lead with action, not analysis. Show the task list first, then offer context.
- If something is blocked, help brainstorm unblocking moves.
- When you spot tasks that don't connect to strategy, mention it briefly — don't lecture.
- When referencing a strategy, use markdown links with the `urls.detail` URL from tool responses.
- Keep the briefing to something scannable in 30 seconds. Details come when asked.
