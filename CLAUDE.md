# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## ⚠️ QUICK START: Workflow for Ticker Analysis

**For any `analyze TICKER` request (MANDATORY SEQUENCE):**
1. **Read WORKFLOW-CHECKLIST.md first** ← Primary execution checklist with template references
2. **Read AGENT.md** ← Detailed workflow steps and freshness rules
3. **Identify the sector** ← Determine which sector the ticker belongs to
4. **Read the sector-specific TEMPLATE and CHECKLIST** ← This defines what metrics, sections, and analysis depth matter
   - Example: For gold/silver miners, read `05_templates/gold_silver_miners_company_template.md` and `05_templates/gold_silver_miners_research_checklist.md`
5. **Use this file (CLAUDE.md)** ← Repository structure, naming conventions, and currency rules

**All five steps are required BEFORE any analysis or file updates.** Missing the sector template/checklist (step 4) results in incomplete or incorrectly structured analysis. The template defines what a complete analysis looks like for that sector.

**For any command-style prompt, check `COMMANDS.md` first.**
Examples:

- `full TICKER`
- `update watchlist`
- `update porto`
- `update macro`

---

## Instruction Priority

For ticker-analysis and company research tasks, treat `AGENT.md` as the primary workflow specification in this repository.

Default behavior:

- Read `AGENT.md` first for any request like `analyze EXK`, `update MTDR`, or `review HL`
- Read `AGENT.md` first for pasted macro screenshots or TradingView calendar images
- Use `CLAUDE.md` as the companion file for repository structure, naming conventions, and supporting workflow details
- Use `COMMANDS.md` as the central index for reusable repo commands
- If the prompt looks like a named repo command, resolve it through `COMMANDS.md` first
- If `CLAUDE.md` and `AGENT.md` overlap, prefer the more specific workflow in `AGENT.md`

## Financial Freshness Rule

For any request involving:

- current market prices
- latest earnings
- macro calendar events
- analyst rating changes
- guidance changes
- SEC filings
- news, catalysts, or management commentary
- "today", "latest", "recent", "this week", or "current"

do not rely on model memory alone.

Always verify with current sources before writing conclusions. Treat cached model knowledge as background only, not as evidence.

Minimum behavior:

- Check the current date first and use explicit dates in the write-up
- Verify the latest reported quarter before referencing financial results
- Verify whether a newer filing, press release, or earnings deck exists
- Verify current price data and label it with `current_price_timestamp`
- Verify whether a cited macro event has already occurred, is upcoming, or has been revised
- If fresh data cannot be verified, say so clearly and avoid presenting stale information as current

Preferred source order for current financial facts:

1. Company investor relations site
2. SEC or official exchange filings
3. Official earnings presentation / shareholder letter
4. Federal Reserve, BLS, BEA, Treasury, EIA, or other primary macro source
5. High-quality market data pages for quote checking

When a prompt asks for a "latest" or "current" view, the agent should assume stale information is risky and should actively refresh the facts.

## Repository Overview

This is an **investing journal** for tracking research and decisions on U.S. stocks. It's a markdown-based knowledge management system focused on five sectors:
- Gold and silver miners
- Oil and energy companies
- Technology companies
- Automotive companies
- Solar energy companies

The repository uses a disciplined folder structure to organize research by theme, sector, and company ticker.

## Folder Structure

- `00_inbox/` — quick captures, links, screenshots, and unprocessed ideas
- `01_strategy/` — investing rules, portfolio goals, risk limits, and decision framework
- `02_market/` — top-down market, macro, and commodity notes
- `03_sectors/` — sector-specific research and company analysis organized by sector
  - `03_sectors/[sector]/companies/` — individual company analysis files (named `TICKER-company-name.md`)
  - `03_sectors/[sector]/earnings/` — earnings reports, transcripts, and reviews (named `YYYY-QN-TICKER.md`)
  - `03_sectors/[sector]/screeners/` — sector watchlists and screening notes
- `04_portfolio/` — holdings, transactions, and watchlist tracking (CSV files)
- `05_templates/` — reusable markdown templates for new analyses
- `06_reviews/` — weekly, monthly, and quarterly review notes
- `07_archive/` — old notes or inactive ideas

## Working with Tickers

When a user provides a **ticker symbol** in Claude chat:

- If the user says `full TICKER`, treat it as a command to run the complete workflow with all templates, checklists, comparison-note steps, and watchlist updates without skipping any required step
- If the user says `analyze TICKER`, default to the same full workflow unless they explicitly ask for a lighter pass
- If the user says `update watchlist`, treat it as a command to refresh current prices in `04_portfolio/watchlist/watchlist.csv`, compare them against `target_buy_zone`, and identify which names are close enough to entry or need re-analysis first
- If the user says `update porto`, treat it as a portfolio-maintenance command focused on `04_portfolio/holdings/holdings_snapshot.csv`, `04_portfolio/transactions/open_orders.csv`, and `04_portfolio/transactions/transactions.csv`

1. **Find the company file**: Look for `03_sectors/[sector]/companies/TICKER-*.md`
   - Example: `03_sectors/oil_energy/companies/MTDR-matador-resources.md`
   - Use `find` or `grep` to locate a ticker across the repository

2. **Check the watchlist row first**: Look in `04_portfolio/watchlist/watchlist.csv`
   - Use `exchange`, `reference_price`, and `last_analyzed_at` as the starting market reference
   - If the ticker is already tracked, use that exchange and price context in the analysis
   - Use the primary listed exchange to set stock-price currency consistency:
     - U.S.-listed names on `NYSE` / `NASDAQ` should use `USD ($)` for stock price, entry zone, price targets, and short-term trading ranges
     - Company-reported financials and guidance may remain in local reporting currency, but include `USD` alongside when useful for consistency in U.S.-listed names

3. **Find earnings files**: Look in `03_sectors/[sector]/earnings/` for `YYYY-QN-TICKER.md`
   - Example: `03_sectors/gold_silver_miners/earnings/2026-Q1-CDE.md`

4. **Update CSV tracking files**:
   - `04_portfolio/watchlist/watchlist.csv` — add/update companies being watched
   - `04_portfolio/holdings/holdings_snapshot.csv` — update current holdings
   - `04_portfolio/transactions/transactions.csv` — log buy/sell transactions

5. **Refresh current facts before analysis**:
   - Check TradingView `Documents` first for the latest earnings release, presentation, and filing links before searching other sources
   - Confirm the latest reported quarter from current filings or the IR site
   - Confirm whether a newer earnings release, presentation, or 8-K exists
   - Confirm current price and the date/time attached to that price
   - Confirm any material news since the last note update
   - Use exact dates such as `2026-05-14`, not vague wording alone

6. **Identify the sector**: Common sectors in use are:
   - `gold_silver_miners`
   - `oil_energy`
   - `technology`
   - `automotive`
   - `solar_energy`

## Working with Pasted Macro Screenshots

When a user pastes a screenshot from TradingView or a similar platform, especially a U.S. economic calendar image:

1. **Extract the visible event data**:
   - date
   - event name
   - time
   - actual
   - forecast
   - prior

2. **Translate the screenshot into macro interpretation**:
   - Is the print hotter, cooler, or in line?
   - Is it supportive or negative for yields?
   - Is it supportive or negative for DXY?
   - Is it supportive or negative for gold and silver?

3. **Create a markdown note** under:
   - `02_market/macroeconomy/`

4. **Focus the note on cross-asset impact**, especially:
   - gold
   - silver
   - DXY
   - U.S. 2Y yield
   - U.S. 10Y yield
   - risk sentiment
   - oil if relevant

5. **Connect the macro read back to tracked sectors**:
   - precious metals miners
   - oil and energy
   - any highly exposed watchlist names

## Key Naming Conventions

- **Company analysis files**: `TICKER-company-name.md` (all lowercase except ticker)
  - Example: `mtdr-matador-resources.md`, `cde-coeur-mining.md`
- **Earnings reviews**: `YYYY-QN-TICKER.md` where QN is Q1–Q4
  - Example: `2026-Q1-MTDR.md`
- **Reviews**: `YYYY-MM-DD.md` (weekly) or `YYYY-MM.md` (monthly)
- **CSV files**: Column headers are lowercase with underscores (e.g., `ticker`, `status`, `current_price_timestamp`, `next_review_scheduled_at`)

## Company Analysis Template

When creating or updating company research, use:

- `AGENT.md` for the default analyze-by-ticker workflow
- `05_templates/company_analysis_template.md` as the generic fallback
- the sector-specific template in `05_templates/` whenever one exists

Primary sector templates:

- `05_templates/gold_silver_miners_company_template.md`
- `05_templates/oil_energy_upstream_ep_company_template.md`
- `05_templates/technology_company_template.md`
- `05_templates/automotive_company_template.md`
- `05_templates/solar_energy_company_template.md`

Generic company structure:

- **Snapshot** — sector, market cap, current price
- **Business Summary** — what the company does, why it matters
- **Thesis** — bull/base/bear cases
- **Why This Could Work** — key catalysts
- **Key Risks** — downside scenarios
- **Financial Quality** — revenue/margin/cash flow trends
- **Valuation** — methods, comparables, price targets
- **What To Monitor** — earnings dates, macro factors, red flags
- **Decision** — conviction level and next review date

Currency consistency:

- Use the stock's primary listing currency for stock-price discussion
- For `NYSE` / `NASDAQ` names, keep stock-price references in `USD ($)`
- For non-U.S. reporting companies that file results in another currency, keep reported financials in local currency and add `USD` alongside when helpful

Also use:

- `05_templates/earnings_comparison_template.md` for three-quarter comparison notes
- the matching sector research checklist in `05_templates/` to decide what metrics matter most
- for China EV automotive names, check `https://cnevpost.com/` for monthly delivery data and model-level sales breakdown to build a trend comparison alongside earnings
- for China EV automotive names, check `https://carnewschina.com/` for new model launches, facelifts, pricing changes, and product-spec updates that may explain delivery trends
- for pre-earnings work, include a short prediction for the coming report and a short-term price scenario with reasons, especially for China EV names where monthly deliveries and product-cycle news can change expectations before the print

## CSV Data Structure

### Watchlist (`04_portfolio/watchlist/watchlist.csv`)
Columns: `ticker`, `company`, `status`, `current_price`, `current_price_timestamp`, `target_buy_zone`, `entry_idea`, `theme`, `thesis_summary`, `reference_price`, `last_analyzed_at`, `next_review_scheduled_at`, `exchange`, `sector`
- Use `status` values: `Watching`, `Buy`, `Hold`, `Sell`, `Archive`
- Use `exchange` as the primary listing reference for future analysis
- Use `current_price` for the latest refreshed market price snapshot
- Use `current_price_timestamp` to record when the latest watchlist price refresh was captured, using Jakarta local time with hour, minute, and second
- Use `reference_price` as the anchored price from the most recent full analysis
- Update `last_analyzed_at` when a `full TICKER` workflow or equivalent full analysis is completed
- Do not overwrite `reference_price` during a routine `update watchlist` refresh unless a fresh full analysis is also being completed
- Update `next_review_scheduled_at` dates regularly

When running `update watchlist`:

- refresh `current_price` and `current_price_timestamp` for active watchlist rows
- compare the updated price against `target_buy_zone`
- summarize which names are:
  - in entry range now
  - near entry range and worth reviewing soon
  - stale and should be re-analyzed before any entry decision

### Holdings (`04_portfolio/holdings/holdings_snapshot.csv`)
Tracks current positions, current price, and unrealized P/L.

### Transactions (`04_portfolio/transactions/transactions.csv`)
Logs all buy/sell trades with date, ticker, shares, price, and notes.

### Open Orders (`04_portfolio/transactions/open_orders.csv`)
Tracks pending buy/sell orders that have not filled yet.

## Common Workflows

### Analyze a New Company by Ticker
When asked to "analyze" a ticker, this is a complete research workflow with multi-quarter trend analysis:

**Step 0: Refresh the timeline**
- Confirm the current date
- Confirm the latest available quarter and report date
- Check whether earnings are already out, merely scheduled, or still upcoming
- Check whether any material event happened after the latest quarter: guidance change, acquisition, offering, operational update, analyst day, or macro shock
- If there is a very recent event, mention it near the top of the analysis so the note does not read as stale

**Step 1: Download Multiple Quarters of Earnings Data**
- Download the **3 quarters of earnings reports** for cyclical stocks (commodities, mining, energy, auto):
  - Current quarter + Prior quarter + Same quarter prior year
  - Example for EXK Q1 2026: Get Q1 2026 + Q4 2025 + Q1 2025 earnings
- Extract key data: Financial metrics, guidance, production/operational targets, management commentary from each quarter
- Follow the storage and output rules in `AGENT.md`
  
**Earnings File Structure:**
Create a **ticker-specific earnings folder** to organize all quarterly reports:
```
03_sectors/gold_silver_miners/earnings/[TICKER]/
├── [YYYY-QN]-comparison.md          ← Multi-quarter comparison & analysis (MAIN FILE)
├── [YYYY-QN]-[TICKER].md            ← Individual quarter transcript/notes (optional)
└── [YYYY-QN]-[TICKER].md            ← Individual quarter transcript/notes (optional)
```

**Example for EXK:**
```
03_sectors/gold_silver_miners/earnings/EXK/
├── 2026-Q1-comparison.md            ← Contains Q1 2026 + Q4 2025 + Q1 2025 comparison
└── 2026-Q1-EXK.md                   ← Individual Q1 2026 details (if needed)
```

**Example for AG:**
```
03_sectors/gold_silver_miners/earnings/AG/
├── 2026-Q1-comparison.md            ← Contains Q1 2026 + Q4 2025 + Q1 2025 comparison + guidance
└── 2026-Q1-AG.md                    ← Individual Q1 2026 details (if needed)
```

**Step 1.5: Fetch TipRanks Analyst Ratings**
- Pull the **5 latest analyst ratings** from TipRanks for the ticker
  - Visit: `https://www.tipranks.com/stocks/[TICKER]` or search TipRanks for rating consensus
  - Extract: Most recent rating date, analyst name/firm, rating (Buy/Hold/Sell), price target, and consensus rating
  - Include in earnings comparison file: Current analyst consensus, price target range, and how current stock price compares to consensus targets
  - Note if consensus is shifting (bullish trend = more upgrades, or bearish trend = more downgrades)
  - Verify the rating dates so old ratings are not presented as new

**Step 2: Compare Across Quarters**
- **Quarter-over-quarter (QoQ):** Compare current vs. prior quarter to identify momentum/trends
  - Revenue growth, margin expansion/compression, production ramps, cost trends
- **Year-over-year (YoY):** Compare same quarter to previous year to identify cyclical patterns
  - Seasonal strength/weakness, commodity price environment changes, operational improvements
- **Trend analysis:** Identify if metrics are improving, deteriorating, or stabilizing
- Create comparison table in earnings file showing all three periods side-by-side

**Step 3: Find or Create Company File**
- Locate `03_sectors/[sector]/companies/TICKER-name.md`
- Read the watchlist row first and carry forward the tracked `exchange`, `reference_price`, and `last_analyzed_at`

**Step 4: Update with Current Data & Trends**
- Populate Snapshot, Financial Quality, What To Monitor, and Decision sections with:
  - Latest earnings stats from most recent quarter
  - **Trend analysis** showing QoQ and YoY momentum
  - Guidance for current year and forward guidance
  - Cost/production trajectory (improving or deteriorating?)
  - Cyclical patterns if relevant (seasonal weakness/strength)
  - Any important post-earnings event that changes the current setup

**Step 5: Create/Update Earnings Files in Ticker Folder**
- Create a **ticker-specific folder** at `03_sectors/[sector]/earnings/[TICKER]/`
- Save the **main comparison file** as `[YYYY-QN]-comparison.md` (e.g., `2026-Q1-comparison.md`)
- This file is the single source of truth for all multi-quarter analysis

**Main Comparison File Structure (`[YYYY-QN]-comparison.md`):**
Include all of the following, using `05_templates/earnings_comparison_template.md`:

1. **Executive Summary**
   - Overall thesis and whether execution is validating

2. **Multi-Quarter Comparison Table**
   - Current Quarter (e.g., Q1 2026)
   - Prior Quarter (e.g., Q4 2025)
   - Prior Year Same Quarter (e.g., Q1 2025)
   - Key Metrics: Revenue, EPS, Production, AISC/costs, Cash flow, Margin trends
   - QoQ and YoY trend indicators (↑ ↓ ↔)

3. **Trend Analysis Section**
   - **Quarter-over-Quarter Momentum:** What changed from last quarter?
   - **Year-over-Year Patterns:** Cyclical strength/weakness identified?
   - **Cost/Production Trajectory:** Improving or deteriorating?

4. **Management Guidance**
   - Current year guidance (revenue, production, costs)
   - Forward guidance (next year if provided)
   - Any changes to previous guidance

5. **Analyst Ratings & Street Consensus**
   - 5 most recent analyst ratings (date, firm, rating, price target)
   - Consensus rating and average price target
   - Current stock price vs. consensus target (upside/downside %)
   - Rating momentum (upgrades/downgrades trend)

6. **Investment Implications**
   - How multi-quarter trend validates or challenges thesis
   - Updated bull/base/bear cases
   - Key risks to monitor

**Optional:** Save individual quarter transcripts as `[YYYY-QN]-[TICKER].md` for reference, but the comparison file is primary.

**Step 6: Analyze Thesis vs. Reality**
- Compare multi-quarter trends against previous thesis
- Identify if execution is accelerating, decelerating, or stalling
- Update bull/base/bear cases based on momentum and guidance
- Flag any cyclical concerns or strengths

**Step 7: Add to Watchlist CSV**
- Include ticker with updated thesis summary that reflects **trend direction**
  - Example: "Strong QoQ momentum; costs declining YoY; production guidance raised"
- Always populate or refresh `current_price`, `current_price_timestamp`, `reference_price`, and `last_analyzed_at`

**Step 8: Set Next Review Date**
- Typically 5-10 business days before next earnings
- For cyclical stocks: Schedule review before seasonal inflection points if applicable

### Update Watchlist Prices

When asked to `update watchlist`:

1. Open `04_portfolio/watchlist/watchlist.csv`
2. Refresh current prices for active names and update `current_price` and `current_price_timestamp`
   - store `current_price_timestamp` as Jakarta local time using `YYYY-MM-DD HH:MM:SS`
   - do not append the timezone label inside the watchlist CSV
3. Compare each current price with `target_buy_zone` and `entry_idea`
4. Group names into:
   - in target zone now
   - near target zone
   - not actionable yet
5. Flag any ticker that should be re-analyzed because the thesis may be stale after earnings, guidance, financing, or macro changes
6. Update `next_review_scheduled_at` if the catalyst calendar has changed
7. Return a short decision-oriented summary so the user can see which names are closest to entry

### Update Macro

When asked to `update macro`, treat it as a top-down market workflow rather than a company workflow.

1. Review current geopolitical developments that could affect inflation, growth, energy, sanctions, trade flows, or supply chains
2. Refresh the current levels and recent trend for:
   - DXY
   - U.S. 2Y yield
   - U.S. 10Y yield
   - real yields if relevant
   - gold
   - silver
   - WTI and Brent
   - other portfolio-relevant commodities if needed
3. Determine whether the current setup is supportive, neutral, or hostile for:
   - the U.S. dollar
   - bond yields
   - precious metals
   - oil and cyclical risk appetite
4. Review the next important U.S. economic releases and policy events:
   - CPI
   - PPI
   - NFP / unemployment
   - retail sales
   - ISM PMI
   - FOMC / Fed speakers
   - Treasury auctions when yields are the main market driver
5. Save a structured note under `02_market/macroeconomy/`
   - Prefer filenames like `YYYY-MM-DD-macro-top-down-review.md`
   - Use `05_templates/macro_top_down_review_template.md`
6. Set the next review date based on the nearest catalyst that could realistically change the view
   - default to the next major U.S. release if it is within a few trading days
   - bring the review forward immediately for major geopolitical escalation or a sharp move in DXY, yields, gold, silver, or oil
7. End with a practical verdict and identify which tracked sectors or watchlist names are most exposed

### Update Porto

When asked to `update porto`, treat it as a portfolio-record workflow.

1. Update `04_portfolio/holdings/holdings_snapshot.csv` for current live positions
2. Update `04_portfolio/transactions/open_orders.csv` for pending orders that are still open
3. Update `04_portfolio/transactions/transactions.csv` for completed fills
4. Keep positions, open orders, and completed trades separated cleanly
5. Use exact dates in all portfolio files

### Analyze a Pasted U.S. Calendar Screenshot

When the user pastes a calendar screenshot, treat it as a request to convert event data into market interpretation.

**Step 1: Extract visible event details**
- Capture the date, time, event name, actual, forecast, and prior values from the screenshot

**Step 2: Determine the macro surprise**
- Compare actual versus forecast
- Note whether the result is inflationary, disinflationary, growth-positive, growth-negative, or neutral

**Step 3: Write the market impact logic**
- Explain the likely effect on:
  - gold
  - silver
  - DXY
  - U.S. 2Y and 10Y yields
  - equities / risk sentiment
  - oil if relevant

**Step 4: Save a markdown note**
- Save under `02_market/macroeconomy/`
- Prefer filenames like:
  - `YYYY-MM-DD-us-calendar-impact.md`
  - `YYYY-MM-DD-macro-screenshot-analysis.md`

**Step 5: End with a usable trend verdict**
- State whether the setup is bullish, neutral, or bearish for gold, silver, DXY, and yields
- Mention the next report or catalyst that matters most

### Log a Watchlist Update
1. Open `04_portfolio/watchlist/watchlist.csv`
2. Add a row or update the existing row for the ticker
3. Update `current_price`, `current_price_timestamp`, `reference_price`, `last_analyzed_at`, `status`, `thesis_summary`, and `next_review_scheduled_at` columns

### Archive an Earnings Report
1. Save earnings transcript/presentation to `03_sectors/[sector]/earnings/`
2. Name it `YYYY-QN-TICKER.md` or similar
3. Consider creating/updating an earnings review file with key insights

### Add Market or Macro Notes
1. Place quick notes in `02_market/` (e.g., `us_equity_market.md`, `commodity_tracker.md`, `macro_notes.md`)
2. Link to specific company analysis when relevant
3. If the user pasted a screenshot, convert it into a structured markdown note rather than leaving it as an unprocessed image

## Helper Commands

- **Find a ticker across the repo**: `grep -r "TICKER" /Users/hf/project/yan/trading`
- **List all companies in a sector**: `ls /Users/hf/project/yan/trading/03_sectors/[sector]/companies/`
- **Search for text across company files**: `grep -r "search term" /Users/hf/project/yan/trading/03_sectors/*/companies/`

## Integration Notes

When Claude Code or agents receive a ticker symbol:
- Check the watchlist row first for tracked exchange and reference price
- Always locate or create the corresponding company file
- Update the watchlist CSV to reflect current status and thinking
- Cross-reference sector trends from the appropriate sector folder
- Link related analysis (macro, commodity prices, sector dynamics)
- Set clear next review dates to maintain a disciplined research cycle

When Claude Code or agents receive `full TICKER`:
- follow the complete checklist in `WORKFLOW-CHECKLIST.md`
- read the sector template and research checklist before writing
- create or update both the comparison note and company note
- do not skip any mandatory workflow step

When Claude Code or agents receive a pasted TradingView macro screenshot:
- Extract the visible event and market data
- Convert it into a markdown note in `02_market/macroeconomy/`
- Explain likely impact on gold, silver, DXY, and yields first
- Connect the macro setup to gold miners, silver miners, and energy names when relevant

This structure enables Claude instances to systematically track, analyze, and update investment research while maintaining consistency across the knowledge base.
