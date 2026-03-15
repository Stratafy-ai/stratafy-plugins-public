# /stratafy-fd:coa-setup

Design a strategy-aligned chart of accounts from scratch, or restructure an existing one to connect every account to strategic intent.

## When to Use

- Setting up a new company's financial structure in Stratafy
- Importing and restructuring an existing COA from Xero/QuickBooks/Sage
- After a major strategy pivot that requires financial restructuring
- When the current COA doesn't tell a strategic story

## Input

Provide one of:
- **A spreadsheet link** — I'll import and restructure it
- **Your accounting system name** — I'll design a COA based on your industry and strategy
- **Nothing** — I'll read your strategy tree and design a COA from first principles

## Process

### Step 1: Understand the Strategic Landscape
I'll pull the full strategic context:
- `get_workspace_snapshot` — Company context, industry, stage
- `get_strategy_tree` — All strategies and their priorities
- `list_initiatives` — Active commitments with budgets

### Step 2: Design or Import the COA

**If designing from scratch:**
- Analyse the strategy tree to identify required financial categories
- Design account structure that groups by strategic function
- Create header accounts for each major strategic area
- Create postable accounts under each header

**If importing:**
- Use `import_sheet_as_finance_proposal` with your spreadsheet
- Review the imported structure for strategic gaps
- Propose restructuring to align with the strategy tree

### Step 3: Create the Proposal
- `create_finance_proposal` — With a strategic narrative explaining the design philosophy
- `create_finance_account` or `bulk_create_finance_accounts` — Add all accounts

### Provenance Context
For every mutation in this setup, include:
- `_source_plugin`: "stratafy-fd"
- `_source_command`: "coa-setup"
- `_change_reasoning`: Brief explanation (e.g. "Designing strategy-aligned COA — creating revenue accounts mapped to GTM strategy")

### Step 4: Map to Strategies
For every postable account:
- `create_finance_mapping` — Connect to the strategy it serves
- Include weight and rationale for each mapping
- Flag any accounts that can't be mapped (investigate these)

### Step 5: Review Together
Present the complete COA with:
- Account hierarchy (visual tree)
- Strategy mapping coverage (% mapped)
- Any gaps or concerns
- Recommended next steps

## Output

A complete finance proposal in Stratafy containing:
- Structured account hierarchy with codes, names, and types
- Strategy mappings for every postable account
- Strategic narrative explaining the design philosophy
- Coverage score and gap analysis

## For the FD
I'll design a COA that passes your scrutiny — proper account coding, clean hierarchy, correct types. Then I'll add the strategic layer on top. You'll see your standard COA structure plus a strategy mapping you've never had before.

## For the Finance Team
I'll walk you through each section of the chart of accounts and explain why it's structured this way. By the end, you'll understand not just where to post transactions, but why each account exists in the context of the company's strategy.
