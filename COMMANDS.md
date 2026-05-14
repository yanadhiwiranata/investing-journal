# Commands

Use this file as the central index for repo commands.

When new commands are added, document them here first so they are easy to search.

## Quick Command List

Use this section for fast copy-paste.

- `full TICKER` — run the full research workflow for a ticker
- `update watchlist` — refresh watchlist prices and flag actionable names
- `update macro` — review geopolitics, DXY, yields, commodities, and the next key U.S. macro date

## Command Details

## Core Commands

### `full TICKER`

Purpose:

- Run the strict full research workflow for a ticker.

What it should do:

- read the required workflow docs and checklists
- identify the sector and use the correct template
- refresh current facts before analysis
- create or update the earnings comparison note
- create or update the company note
- update the watchlist entry when appropriate
- set the next review date

Primary references:

- `AGENT.md`
- `WORKFLOW-CHECKLIST.md`
- `CLAUDE.md`

## Watchlist Commands

### `update watchlist`

Purpose:

- Refresh current watchlist prices and identify which names are actionable or need re-analysis.

What it should do:

- update `current_price`
- update `current_price_timestamp` with full Jakarta-time precision
- compare price vs. `target_buy_zone` and `entry_idea`
- flag names in range, near range, or stale
- update `next_review_scheduled_at` if the catalyst calendar changed

Primary references:

- `AGENT.md`
- `WORKFLOW-CHECKLIST.md`
- `CLAUDE.md`

## Macro Commands

### `update macro`

Purpose:

- Run a top-down macro review covering geopolitics, DXY, U.S. bond yields, commodities, and the upcoming U.S. economic calendar.

What it should do:

- review current geopolitical developments
- refresh DXY, 2Y, 10Y, and real-yield context
- refresh gold, silver, oil, and other relevant commodities
- identify the next important U.S. macro release
- save a structured macro note
- set the next review date based on the nearest meaningful catalyst

Primary references:

- `02_market/macroeconomy/update-macro-command.md`
- `05_templates/macro_top_down_review_template.md`
- `AGENT.md`
- `CLAUDE.md`

## Naming Rule For Future Commands

When adding a new command:

1. Add it to this file.
2. Add the detailed workflow to the most relevant domain file.
3. Add any reusable template if the command creates recurring notes.
4. Link the command from `README.md` if it is part of normal usage.
