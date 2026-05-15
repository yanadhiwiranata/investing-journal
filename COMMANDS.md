# Commands

Use this file as the central index for repo commands.

When new commands are added, document them here first so they are easy to search.

## Quick Command List

Use this section for fast copy-paste.

- `full TICKER` — run the full research workflow for a ticker
- `update today` — run the daily maintenance sequence: macro, watchlist, then portfolio
- `update watchlist` — refresh watchlist prices and flag actionable names
- `update porto` — update live positions, open orders, and completed trades
- `update macro` — run an upgraded macro refresh with top-down review, event-risk tracking, and hourly notes when conditions are volatile

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

### `update today`

Purpose:

- Run the standard daily maintenance workflow in the correct order so top-down context is refreshed before watchlist and portfolio maintenance.

What it should do:

- run `update macro` first
- run `update watchlist` second
- run `update porto` third
- preserve the sequence unless the user explicitly asks for a different order
- return one concise summary covering macro changes, watchlist actions, and portfolio record updates

Primary references:

- `AGENT.md`
- `WORKFLOW-CHECKLIST.md`
- `CLAUDE.md`
- `README.md`

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

## Portfolio Commands

### `update porto`

Purpose:

- Update the portfolio records for current positions, open orders, and completed trades.

What it should do:

- update `04_portfolio/holdings/holdings_snapshot.csv` for live positions
- refresh current price and unrealized P/L for held positions
- update `04_portfolio/transactions/open_orders.csv` for pending buy or sell orders
- update `04_portfolio/transactions/transactions.csv` for filled trades
- keep portfolio state aligned between current holdings, pending orders, and completed transactions

Primary references:

- `AGENT.md`
- `CLAUDE.md`
- `README.md`

## Macro Commands

### `update macro`

Purpose:

- Run an upgraded macro workflow covering geopolitics, DXY, U.S. bond yields, commodities, the upcoming U.S. economic calendar, and higher-frequency tracking when markets are volatile.

What it should do:

- review current geopolitical developments
- refresh DXY, 2Y, 10Y, and real-yield context
- refresh gold, silver, oil, and other relevant commodities
- identify the next important U.S. macro release
- save a structured macro top-down note
- add an hourly macro monitor note when volatility, event risk, or cross-asset moves are elevated
- set the next review time based on the nearest meaningful catalyst or the next hourly checkpoint during high-volatility periods

Primary references:

- `05_templates/macro_top_down_review_template.md`
- `05_templates/macro_hourly_monitor_template.md`
- `AGENT.md`
- `CLAUDE.md`

## Naming Rule For Future Commands

When adding a new command:

1. Add it to this file.
2. Add the detailed workflow to the most relevant domain file.
3. Add any reusable template if the command creates recurring notes.
4. Link the command from `README.md` if it is part of normal usage.
