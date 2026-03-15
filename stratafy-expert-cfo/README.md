# Stratafy FD

Your fractional Finance Director — connects every dollar to strategic intent.

## Role-Adaptive Agent

The plugin adapts its persona based on the connected user's role:

| Role | Persona | Voice |
| --- | --- | --- |
| Finance Director / CFO | Peer | Direct, expert, challenges assumptions |
| Finance team member | Coach | Warm, educational, explains the why |

Same skills, same commands — different voice and authority level. The Stratafy MCP handles role detection.

## Commands (6)

| Command | Description |
| --- | --- |
| `/stratafy-fd:coa-setup` | Design a strategy-aligned chart of accounts |
| `/stratafy-fd:financial-scan` | Run financial alignment scans (L1/L2/L3) |
| `/stratafy-fd:budget-mapping` | Connect spend to strategy — map accounts to priorities |
| `/stratafy-fd:quarterly-review` | Comprehensive quarterly financial review |
| `/stratafy-fd:investor-prep` | Prepare financial materials for investors |
| `/stratafy-fd:explain-metric` | Explain a financial metric in strategic context |

## Skills (3)

| Skill | Description |
| --- | --- |
| `financial-intelligence` | COA design, financial scanning, metric tracking |
| `strategy-finance-alignment` | Connecting spend to strategy, mapping, analysis |
| `financial-operations` | Day-to-day finance tasks, reporting, investor prep |

## MCP Servers (2)

Configured in `.mcp.json`: Stratafy, Linear.

## Provenance

On every mutation tool call (create_*, update_*, delete_*, link_*, etc.), always include:
- `_source_plugin`: "stratafy-fd"
- `_source_command`: the command being run (e.g. "coa-setup", "pricing-model")
- `_change_reasoning`: 1-2 sentences explaining WHY this change is being made

The system handles approval automatically based on the user's workspace role.
