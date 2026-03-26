---
description: Connect spend to strategy — review financial account to strategy mappings
---

# /stratafy-expert-cfo:budget-mapping

Connect spend to strategy. Review and improve the mappings between financial accounts and strategic priorities. The goal: every rand should trace to a strategic intent.

## When to Use

- After creating a new COA (follow-up to `/stratafy-expert-cfo:coa-setup`)
- When new accounts have been added without mappings
- After a strategy change that affects financial allocation
- When the financial scan shows low mapping coverage
- During budget planning to ensure allocations match priorities

## Input

Provide one of:
- **A specific strategy** — "map accounts for our Product Architecture strategy"
- **Unmapped only** — "show me accounts without strategy mappings"
- **A specific account or account range** — "map the 5000-series expense accounts"
- **Nothing** — I'll review all mappings and flag gaps

## Process

### Step 1: Get User Context

Call `get_user_context` with `command_name: "budget-mapping"`, `plugin_name: "stratafy-expert-cfo"`.
This returns the user's personal context (chapter, values, forward anchor, lens, role mandate) and logs the session start. Use this context to calibrate your responses throughout the command.

### Step 2: Load Current State
- `get_finance_proposal` — Full COA with accounts
- `list_finance_mappings` — Existing mappings
- `get_strategy_tree` — Strategy hierarchy and priorities

### Step 3: Identify Gaps
- Accounts with no mappings
- Strategies with no financial backing
- Mappings with missing rationale
- Mappings with weights that don't sum correctly

### Step 4: Propose Mappings
For each unmapped account, I'll:
- Analyse the account name, code, type, and description
- Match it against the strategy tree
- Suggest primary and supporting strategies
- Propose weights based on likely allocation
- Draft rationale

### Step 5: Interactive Mapping
I'll present proposals one by one (or in batches):

```
Account: 5210 Cloud Infrastructure (expense, variable)
Proposed mapping:
  → Product Architecture (primary, weight: 0.8)
    "Core infrastructure that the product runs on"
  → Operations (supporting, weight: 0.2)
    "Internal tooling and monitoring infrastructure"

Accept / Modify / Skip?
```

### Step 6: Apply Confirmed Mappings
- `create_finance_mapping` for each confirmed mapping
- Track what was mapped, skipped, or deferred

### Provenance Context
For every mutation in this command, include:
- `_source_plugin`: "stratafy-expert-cfo"
- `_source_command`: "budget-mapping"
- `_change_reasoning`: Brief explanation of why this change is being made

### Step 7: Summary
- Accounts mapped in this session
- Remaining gaps
- Updated alignment score
- Strategies still underfunded or unfunded

## Output

An updated set of strategy-account mappings with:
- Complete rationale for each mapping
- Weight allocations that sum correctly
- Coverage improvement summary
- Remaining gaps flagged for follow-up

## For the FD
I'll move fast — propose mappings in batches, let you approve/reject/modify. We'll close the gap efficiently. I'll flag the politically sensitive ones (spend that doesn't match stated priorities) for your judgment.

## For the Finance Team
I'll explain each mapping so you understand why cloud costs relate to the Product strategy, or why sales commissions map to GTM. This builds your strategic thinking — not just categorisation skills.
