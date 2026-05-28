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
- `update macro china` — run a China-specific macro refresh covering PBOC policy, CNY, PMI, EV sector health (NIO focus), and geopolitical risk; only triggered by this exact phrase
- `entry check gold` — verify real yields + DXY + gold price; decide if gold miner Layer 1 entry is viable
- `entry check silver` — verify ISM + copper + industrial demand; decide if silver miner Layer 1 entry is viable
- `entry check both` — run full decision checklist for both gold and silver miners; recommend which to enter first

---

## Command Details

> **Note:** Detailed workflow steps for each command live in `CLAUDE.md` (Common Workflows section) and `AGENT.md`. This file is an index — purpose and references only.

---

## Core Commands

### `full TICKER`

Purpose: Run the strict full research workflow for a ticker.

Primary references:
- `AGENT.md`
- `WORKFLOW-CHECKLIST.md`
- `CLAUDE.md`

---

### `update today`

Purpose: Run the standard daily maintenance workflow in the correct order — `update macro` first, `update watchlist` second, `update porto` third.

Primary references:
- `AGENT.md`
- `WORKFLOW-CHECKLIST.md`
- `CLAUDE.md`

---

## Watchlist Commands

### `update watchlist`

Purpose: Refresh current watchlist prices and identify which names are actionable or need re-analysis.

Primary references:
- `CLAUDE.md` (Update Watchlist Prices workflow)
- `04_portfolio/watchlist/watchlist.csv`

---

## Portfolio Commands

### `update porto`

Purpose: Update the portfolio records for current positions, open orders, and completed trades.

Primary references:
- `CLAUDE.md` (Update Porto workflow)
- `04_portfolio/holdings/holdings_snapshot.csv`
- `04_portfolio/transactions/open_orders.csv`
- `04_portfolio/transactions/transactions.csv`

---

## Macro Commands

### `update macro`

Purpose: Run a top-down macro refresh covering geopolitics, DXY, U.S. bond yields, commodities, and the upcoming U.S. economic calendar. Add hourly monitor notes when markets are volatile.

**Out of scope:** Do NOT open the watchlist CSV, do NOT pull individual stock prices or ticker data. Macro-sector mentions in the verdict are text-only.

Primary references:
- `CLAUDE.md` (Update Macro workflow)
- `05_templates/macro_top_down_review_template.md`
- `05_templates/macro_hourly_monitor_template.md`

---

### `update macro china`

Purpose: Run a China-specific macro refresh focused on PBOC policy, CNY dynamics, domestic macro prints, EV sector health, and portfolio impact for China-listed names tracked in this repo (currently NIO).

**Trigger rule:** Only triggered by the exact phrase `update macro china`. Do not run as part of `update macro`, `update today`, or any other command.

Primary references:
- `CLAUDE.md` (Update Macro China workflow)
- `05_templates/macro_china_top_down_review_template.md`
- CNEVPost (`https://cnevpost.com/`) for monthly delivery data
- CarNewsChina (`https://carnewschina.com/`) for model/competitive news
- NBS (`http://www.stats.gov.cn/`) for official economic data
- PBOC (`http://www.pbc.gov.cn/`) for monetary policy

---

## Entry Commands (Sector Rotation)

### `entry check gold`

Purpose: Verify whether gold miner Layer 1 entry is viable today based on macro conditions.

What it should do:
- check real yields (FRED DFF10): Must be <+0.5%
- check DXY (TradingView): Must be stable or falling
- check gold price (TradingView): Must be >$4,400
- check analyst consensus (TipRanks): For preferred gold miners (AU, NEM, CDE)
- if all gates pass: recommend Layer 1 entry with specific price targets
- if any gate fails: explain which barrier is blocking entry and when to re-check

Primary references:
- `01_strategy/sector_rotation/gold_miners/rotation_rules.md`
- `01_strategy/sector_rotation/ENTRY_DECISION_CHECKLIST.md`

---

### `entry check silver`

Purpose: Verify whether silver miner Layer 1 entry is viable today based on industrial demand conditions.

What it should do:
- check ISM PMI (FRED or Trading Economics): Must be >50
- check copper price (TradingView HG): Must not be down >2% recent
- check industrial end-markets: Auto/solar production data stable or growing
- check risk sentiment: VIX <25, tech/crypto not collapsing
- check analyst consensus (TipRanks): For preferred silver miners (EXK, HL, AG)
- if all gates pass: recommend Layer 1 entry with specific price and size (50–75 shares)
- if any gate fails: explain which barrier is blocking entry (more gates than gold)

Primary references:
- `01_strategy/sector_rotation/silver_miners/rotation_rules.md`
- `01_strategy/sector_rotation/ENTRY_DECISION_CHECKLIST.md`

---

### `entry check both`

Purpose: Run full decision checklist for both gold and silver miners; recommend which sector to enter first (or skip both).

What it should do:
- run gold gate checks (real yields, DXY, gold price, analyst)
- run silver gate checks (ISM, copper, industrial demand, risk sentiment, analyst)
- if both gates pass: recommend entering GOLD first (100 shares Layer 1), then SILVER separately (50–75 shares)
- if only GOLD passes: recommend GOLD entry only
- if only SILVER passes: note this is rare; proceed with SILVER caution
- if neither passes: recommend SKIP and set re-check date (typically after CPI/PMI releases)
- provide side-by-side comparison of current conditions vs. entry thresholds
- note current position weights (cap gold <40%, silver <25%)

Primary references:
- `01_strategy/sector_rotation/ENTRY_DECISION_CHECKLIST.md`
- `01_strategy/sector_rotation/gold_miners/vs_silver_comparison.md`
- `01_strategy/sector_rotation/gold_miners/rotation_rules.md`
- `01_strategy/sector_rotation/silver_miners/rotation_rules.md`
- `01_strategy/macro_may_2026_rebound_headwind.md` (current conditions)

---

## Naming Rule For Future Commands

When adding a new command:

1. Add it to this file (purpose + primary references only).
2. Add the detailed workflow steps to `CLAUDE.md` (Common Workflows) or `AGENT.md`.
3. Add any reusable template if the command creates recurring notes.
4. Link the command from `README.md` if it is part of normal usage.
