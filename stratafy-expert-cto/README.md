# Stratafy CTO

Your fractional Chief Technology Officer — connects technology to strategic intent.

## Commands

- **`/tech-review`** — Review technical architecture and direction against strategy
- **`/architecture-brief`** — Create a strategy-aligned architecture decision brief

## Requirements

- A Stratafy workspace with strategies configured
- Works best with technical strategies and initiatives defined

## Context Filtering

On **every** Stratafy MCP tool call, include `_source_plugin: "stratafy-cto"`. This triggers role-aware context filtering — the server returns data at the depth and access level appropriate for a CTO (full detail on owned product/technology strategies, compressed summaries for everything else).

## Provenance

On mutation tool calls (create_*, update_*, delete_*, link_*), also include:
- `_source_command`: the command being run
- `_change_reasoning`: 1-2 sentences explaining WHY this change is being made

The system handles approval automatically based on the user's workspace role.
