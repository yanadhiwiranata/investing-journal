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

### `update macro china`

Purpose:

- Run a China-specific macro refresh focused on PBOC policy, CNY dynamics, domestic macro prints, EV sector health, and portfolio impact for China-listed names tracked in this repo (currently NIO).
- **Only triggered by the exact phrase `update macro china`.** Do not run as part of `update macro`, `update today`, or any other command unless the user types this exact command.

What it should do:

1. **Verify live data first (mandatory):** USD/CNY, Hang Seng, HSTECH, CSI 300, NIO price, China 10Y yield, copper, latest NBS/Caixin PMI prints — all with timestamps
2. **Assess PBOC stance:** Current LPR rates, RRR level, recent PBOC actions, CNY fixing trend, M2/credit conditions
3. **Review latest macro prints:** NBS + Caixin PMI (manufacturing and services), CPI, PPI, industrial production, retail sales, property data
4. **EV sector health check:**
   - Pull monthly delivery data from CNEVPost for NIO, BYD, Li Auto, XPEV, AITO
   - Check CarNewsChina for new model launches, price changes, competitive moves
   - Review active EV subsidies and policy changes
   - Assess BYD pricing pressure and HUAWEI AITO competition
5. **Geopolitical & trade risk:** U.S.–China tariffs, EU EV tariffs, Taiwan risk, ADR delisting risk
6. **China economic calendar:** Upcoming NBS/Caixin PMI, CPI/PPI, LPR decision, NIO delivery release dates
7. **Cross-asset implications for NIO:** CNY trend, HSTECH sentiment, PBOC easing cycle, property recovery as wealth-effect proxy
8. **Save note** under `02_market/macroeconomy/` as `YYYY-MM-DD-HHMM-macro-china-top-down.md`
9. **End with a verdict:** Bull/base/bear scenario for NIO over the next 2–4 weeks based on current China macro setup

Primary references:

- `05_templates/macro_china_top_down_review_template.md`
- CNEVPost (`https://cnevpost.com/`) for monthly delivery data
- CarNewsChina (`https://carnewschina.com/`) for model/competitive news
- NBS (`http://www.stats.gov.cn/`) for official economic data
- PBOC (`http://www.pbc.gov.cn/`) for monetary policy

**Important:** This command is China-only. Do not replace or merge with `update macro` (which covers U.S. macro). Run both for a full global picture when analyzing NIO.

---

## Entry Commands (Sector Rotation)

### `entry check gold`

Purpose:

- Verify whether gold miner Layer 1 entry is viable today based on macro conditions.

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

Purpose:

- Verify whether silver miner Layer 1 entry is viable today based on industrial demand conditions.

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

Purpose:

- Run full decision checklist for both gold and silver miners; recommend which sector to enter first (or skip both).

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

1. Add it to this file.
2. Add the detailed workflow to the most relevant domain file.
3. Add any reusable template if the command creates recurring notes.
4. Link the command from `README.md` if it is part of normal usage.
