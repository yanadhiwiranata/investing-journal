# AGENT.md

This file defines how an AI research agent should operate inside this repository.

## Purpose

The repository is an investing research workspace organized around sectors, company notes, earnings archives, and portfolio tracking. The primary agent workflow is:

- User says: `analyze EXK`
- Agent identifies the company and sector
- Agent downloads and saves the latest earnings materials plus the two comparison quarters
- Agent writes a multi-quarter comparison note
- Agent updates or creates the company analysis in the correct sector folder
- Agent uses the sector-specific template instead of forcing all companies into one generic framework

## ⚠️ MANDATORY: Read Before Every Analysis

**Before starting any ticker analysis, consult:**
1. **WORKFLOW-CHECKLIST.md** — step-by-step checklist with template references (PRIMARY REFERENCE)
2. **CLAUDE.md** — repository structure, naming conventions, currency rules
3. **This file (AGENT.md)** — detailed workflow steps, freshness rules, output standards
4. **COMMANDS.md** — central index of reusable repo commands

**Before handling a command-style prompt, check `COMMANDS.md` first.**
Examples:

- `full TICKER`
- `pulse TICKER`
- `update today`
- `update watchlist`
- `update porto`
- `update macro`

**Non-negotiable rules:**
- ✅ Read this file (AGENT.md) first for any `analyze TICKER` request
- ✅ Read `COMMANDS.md` first for any command-style prompt
- ✅ Use WORKFLOW-CHECKLIST.md as your execution checklist
- ✅ Use sector-specific template from `05_templates/` (never generic template if sector template exists)
- ✅ Create/update earnings comparison file with analyst consensus (Step 1.5 below is MANDATORY)
- ✅ Use exact dates (`2026-05-14`, not "last week") in all outputs
- ✅ Tag refreshed market prices with `current_price_timestamp: YYYY-MM-DD HH:MM:SS` in Jakarta local time
- ✅ Update watchlist CSV after analysis
- ✅ **For Indonesian banking tickers (BBCA, BBRI, BMRI):** Read `02_market/macroeconomy/indonesia-macro-economy.md` at the start of analysis. If any BI Rate, OJK policy, KUR quota, or government program has changed since the last Update Log entry, update that file before proceeding with the bank analysis. This is the single source of truth for Indonesian macro context.

**If any of these are skipped, the analysis is incomplete.**

## Freshness Protocol For Financial Work

Model knowledge may be stale for financial markets. For any request involving current conditions, the agent must refresh the facts before answering.

Always treat these as freshness-sensitive:

- latest or current stock price
- earnings dates and reported quarters
- guidance changes
- analyst rating changes
- SEC filings
- macro releases and calendar events
- commodity moves
- breaking company news
- anything phrased as `latest`, `today`, `recent`, `current`, `this week`, or `this month`

Required behavior:

1. Determine the current date before analysis.
2. Verify the latest quarter and report date from current primary sources.
3. Verify whether a newer filing, press release, presentation, or 8-K exists.
4. Verify current price and record the exact `current_price_timestamp` in Jakarta local time.
5. Verify whether major events happened after the latest quarter.
6. Use absolute dates in notes, not only relative phrases.
7. If current data cannot be verified, state that clearly and downgrade confidence.

Do not present remembered or previously cached financial facts as current unless they were refreshed.

Preferred source order:

1. Company investor relations site
2. SEC / official exchange filings
3. Official earnings release, deck, or shareholder letter
4. Primary macro source: BLS, BEA, Fed, Treasury, EIA, company IR event pages
5. Reputable market data page for quote checking

Verify current prices from available sources: SEC filings, earnings releases, company IR pages, and financial data sites. Proceed with analysis using the best available information without blocking on manual data entry.

## Default Command Behavior

If the prompt matches a repo command, resolve it against `COMMANDS.md` first and then follow the linked detailed workflow.

When the user provides only a ticker and a verb such as `analyze`, `review`, or `update`, interpret it as a full research workflow unless the user says otherwise.

When the user says `pulse TICKER`, interpret it as a **lightweight exogenous refresh only**. Do not rebuild fundamentals, rebuild valuation, re-run earnings comparisons, or re-run sector templates. Scope is strictly:

1. **Price drift** — current price vs. `reference_price` in watchlist CSV (% change since last full analysis)
2. **Recent headlines (last 7–14 days)** — press releases, 8-K filings, management commentary, news articles; flag anything that could shift thesis or entry timing
3. **Sector macro backdrop (2–3 indicators only)** — only the indicators that directly drive the sector (e.g., real yields + DXY for gold miners; ISM + copper for silver miners; WTI + EIA for oil; BI rate + IDR for Indonesian banks)
4. **Analyst rating or price target changes** since `last_analyzed_at` — check TipRanks or search; note firm, date, old vs. new
5. **Corporate actions** since last full analysis — secondary offerings, buybacks, insider trades, guidance revisions, M&A

**What pulse CAN update in the company file:**
- `current_price` and `current_price_timestamp` in Snapshot
- **Current Context** — refresh with latest price, headlines, and post-analysis events
- **Thesis** — update bull/base/bear narrative if macro or geopolitical shifts change the story
- **Why This Could Work** — add or revise catalysts if new ones emerge from headlines
- **Key Risks** — add, remove, or reprioritize risks based on new developments
- **What To Monitor** — update watchpoints if new headlines change what matters next
- **Decision** — update conviction level, entry zone timing, next review date

**What pulse cannot touch (requires financial recomputation — leave as-is):**
- Business Summary, Financial Quality, Valuation

**Do not:**
- Re-run the sector template or research checklist
- Redo earnings comparison or financial trend analysis
- Change `reference_price` or `last_analyzed_at` in the watchlist CSV

**Output:**
- Update the decision-relevant sections in the company `.md` file (Current Context, What To Monitor, Decision)
- Append a `## Pulse Update – YYYY-MM-DD` changelog block at the bottom of the company file
- Update only `current_price` and `current_price_timestamp` in the watchlist CSV
- Flag if a `full TICKER` re-run is now warranted

**Flag `full TICKER` as needed when:**
- Earnings miss or beat that materially changes the thesis
- CEO, CFO, or key operator change
- Acquisition, merger, or spinoff announced
- Secondary offering that significantly dilutes
- Macro regime shift that breaks original thesis assumptions

---

When the user says `full TICKER`, interpret it as the strictest version of the full workflow:

- complete every required step in `WORKFLOW-CHECKLIST.md`
- read the matching templates and checklists before analysis
- create or update both the earnings comparison note and company note
- update watchlist fields when appropriate
- do not skip any required checklist or workflow step

Example:

- `analyze EXK`
- `update MTDR`
- `review HL`
- `full CDE`
- `update today`
- `update watchlist`
- `update porto`
- `update macro`

For these requests, assume the user wants the most recent financially relevant information, not just an update to old notes. Refresh the facts first.

When the user says `update today`, interpret it as a sequential maintenance command with this required order and **consistent hourly timestamps across all three steps:**

1. `update macro` (with **MANDATORY Step 0 data freshness verification + HOURLY TIMESTAMP FILENAME**)
2. `update watchlist` (refresh with **matching timestamp**)
3. `update porto` (refresh with **matching timestamp**)

Required behavior for `update today`:

- ⚠️ **BEFORE starting**, execute Step 0 of the Macro Top-Down Review Workflow: Verify current DXY, yields, gold, silver, WTI, and latest macro data releases with live sources. Do NOT rely on cached prices.
- **Use consistent timestamp across all three components** (same HHMM for macro, watchlist, and portfolio)
  
- **For macro file naming:** Create file with hourly timestamp: `YYYY-MM-DD-HHMM-macro-today-update.md`
  - Example: `2026-05-15-0800-macro-today-update.md`
  - If running `update today` multiple times same day, each gets a new HHMM timestamp (don't overwrite)
  - Include Data Freshness Table with 24H Δ and Intraday Δ columns
  - Include Update Log table at bottom

- **For watchlist update:** Refresh `04_portfolio/watchlist/watchlist.csv`
  - Update `current_price` and `current_price_timestamp` with matching time: `2026-05-15 08:00:00`
  - Use macro read to flag any thesis staleness after market moves

- **For portfolio update:** Refresh holdings, open orders, and transactions
  - Update `current_price` with matching timestamp: `2026-05-15 08:00:00`
  - Recalculate unrealized P/L based on refreshed prices

- complete the three commands in that order unless the user explicitly changes it
- ensure all three data sources reflect the same time checkpoint (no stale prices between macro/watchlist/portfolio)
- finish with one concise summary covering macro, watchlist, and portfolio outcomes

Expected output for `update today`:

- macro note with hourly timestamp filename and freshness verification (`2026-05-15-0800-macro-today-update.md`)
- updated watchlist with matching timestamp (`current_price_timestamp = 2026-05-15 08:00:00`)
- updated portfolio with matching timestamp (`current_price_timestamp = 2026-05-15 08:00:00`)
- one combined decision-useful summary that links macro context to watchlist actionability and portfolio P/L impact

When the user says `update watchlist`, interpret it as a watchlist-maintenance command focused on `04_portfolio/watchlist/watchlist.csv`, not as a single-ticker deep dive.

Required behavior for `update watchlist`:

- refresh the current price for each active watchlist name
- update `current_price` and `current_price_timestamp` with the latest verified market data
- compare the refreshed price against `target_buy_zone` and `entry_idea`
- identify which names are:
  - already in or near the target buy zone
  - close enough that they deserve a fresh re-analysis before entry
  - too extended or too far away to matter right now
- keep exact dates in the CSV and in the summary
- if a ticker has had a material earnings, guidance, financing, or macro change since the last note, explicitly flag it for re-analysis instead of using price alone

Expected output for `update watchlist`:

- updated `watchlist.csv`
- a concise summary of:
  - names entering target zones
  - names approaching target zones
  - names that should be re-analyzed before any entry decision
  - names with stale thesis or stale `next_review_scheduled_at` timing

When the user says `update porto`, interpret it as a portfolio-maintenance command focused on:

- `04_portfolio/holdings/holdings_snapshot.csv`
- `04_portfolio/transactions/open_orders.csv`
- `04_portfolio/transactions/transactions.csv`

Required behavior for `update porto`:

- update current live positions in `holdings_snapshot.csv`
- refresh position-level metrics in `holdings_snapshot.csv` such as current price, unrealized P/L, and unrealized P/L %
- **do not include tickers with 0 shares in `holdings_snapshot.csv`** — only active positions
- update open pending orders in `open_orders.csv`
  - **sort by: sector (asc), ticker (asc), action (asc), limit_price (asc)**
- update filled trades in `transactions.csv`
  - **sort by: sector (asc), ticker (asc), action (asc), price (asc)**
- keep open orders separate from completed transactions
- use exact dates in all portfolio files
- include `sector` column in all portfolio CSV files (holdings, open_orders, transactions)

Expected output for `update porto`:

- updated portfolio CSV files as needed
- a concise summary of:
  - current live positions changed
  - open orders added, edited, or removed
  - completed transactions logged

When the user pastes a macro or calendar screenshot, interpret it as a macro-impact workflow unless the user says otherwise.

Examples:

- pasted TradingView U.S. economic calendar screenshot
- pasted screenshot of yields, DXY, gold, and silver
- `analyze this calendar`

When the user asks to `update macro`, interpret it as an upgraded macro workflow, not a light check-in. It should cover geopolitics, DXY, U.S. bond yields, commodity trends, the next important U.S. calendar catalysts, and higher-frequency monitoring when the market is volatile.

**⚠️ HOURLY MACRO UPDATE PROTOCOL:**

For active macro monitoring days (e.g., around major data releases, geopolitical shocks, or high volatility), support **hourly intraday updates** using timestamp-based filenames:

**⚠️ Get real system time first (MANDATORY):**
- Run `date` in the terminal BEFORE naming any macro file
- The system clock is already set to WIB (Jakarta time); `date` gives the only authoritative timestamp
- Example: `date` → `Thu May 28 17:36:00 WIB 2026` → use `1736` in filename, `17:36` in header
- **Never guess, estimate, or invent a time.** A fabricated timestamp invalidates the entire audit trail.

**File Naming Convention:**
- Each hour gets a NEW file with timestamp in filename: `YYYY-MM-DD-HHMM-macro-[TYPE].md`
- **First update of the day:** `2026-05-15-0600-macro-top-down-review.md`
- **Subsequent hourly updates:** `2026-05-15-0700-macro-update.md`, `2026-05-15-0800-macro-update.md`, etc.
- This prevents confusion about which file is current (always look for the latest HHMM timestamp)
- Keep ALL hourly versions in `02_market/macroeconomy/` for complete audit trail

**Required Content in Each Hourly File:**
1. **Header with timestamp:** `**Last Updated:** 2026-05-15 07:00 Jakarta time` (include HH:MM)
2. **Data Freshness Table** with three columns:
   - **Level:** Current market price/yield
   - **24H Δ:** Change since previous day close
   - **Intraday Δ:** Change since market open (or since previous hour if tracking sub-hourly)
3. **Update Log Table** at bottom showing each hourly refresh:
   - Time (Jakarta) → Update summary → Key changes made
4. **Macro Regime Assessment:** Current thesis and which assets are under pressure/supported

**When to Create New Hourly File:**
- Every 60 minutes if monitoring actively
- Immediately if: DXY breaks ±2%, gold/silver/oil move ±2%, yields move ±20 bps, geopolitical news breaks, Fed speaker releases comments
- On every major economic data release (NFP — Non-Farm Payrolls; CPI — Consumer Price Index; PPI — Producer Price Index; PCE — Personal Consumption Expenditures, Fed's preferred inflation gauge; jobless claims; etc.)

**What NOT to do:**
- Do NOT overwrite previous hourly files
- Do NOT rely on memory alone for prices; refresh with live sources every hour
- Do NOT mix daily and hourly files in the same document (keep hourly versions separate)

## Macro Screenshot Workflow

Use this workflow when the user pastes a screenshot from TradingView or a similar platform showing:

- U.S. economic calendar events
- bond yields
- DXY
- gold
- silver
- oil
- other macro indicators relevant to market direction

### 1. Read the screenshot

Extract the visible information from the image, including:

- date
- time
- event name
- country
- actual
- forecast
- prior

If the screenshot includes market prices, also extract:

- DXY
- U.S. 2Y and 10Y yields
- gold
- silver
- oil
- VIX
- USDIDR or other currency references if shown

### 2. Interpret the macro signal

For each event, determine:

- whether the result is hotter, cooler, or in line versus forecast
- whether it is growth-positive, inflation-positive, or risk-negative
- whether it is likely bullish or bearish for yields
- whether it is likely bullish or bearish for DXY
- whether it is likely bullish or bearish for gold and silver

Always separate:

- immediate reaction logic
- medium-term trend implications

### 3. Create a macro markdown note

Save the analysis under:

- `02_market/macroeconomy/`

Recommended filenames:

- `YYYY-MM-DD-us-calendar-impact.md`
- `YYYY-MM-DD-macro-screenshot-analysis.md`

### 4. Cover the required impact areas

The markdown note should explicitly analyze impact on:

- Gold
- Silver
- DXY
- U.S. 2Y yield
- U.S. 10Y yield
- Real yields if relevant
- Risk sentiment / equities
- Oil if relevant
- Mining stocks if relevant

### 5. Produce a trend verdict

End the note with a practical trend read:

- bullish / neutral / bearish for gold
- bullish / neutral / bearish for silver
- bullish / neutral / bearish for DXY
- bullish / neutral / bearish for yields

Also state:

- what matters most next
- what upcoming report could confirm or invalidate the move
- what miners or sectors are most sensitive to the setup

### 6. Link the macro note to sector research when relevant

If the macro read matters for active sectors in the repo, link it mentally and in the written note to:

- `03_sectors/gold_miners/`
- `03_sectors/silver_miners/`
- `03_sectors/oil_energy/`

## Macro Note Output Standard

When working from a pasted macro screenshot, the markdown note should include these sections:

1. Screenshot Summary
2. Event Table
3. What Beat / Missed vs Forecast
4. Market Impact Logic
5. Gold Impact
6. Silver Impact
7. DXY and Yield Impact

## Macro Top-Down Review Workflow

Use this workflow when the user asks for:

- `update macro`
- a geopolitics, DXY, yields, and commodities refresh
- a top-down market review
- help deciding when the next macro review should happen

### ⚠️ STEP 0: DATA FRESHNESS VERIFICATION (MANDATORY — DO THIS FIRST)

**This step is non-negotiable for macro updates.** Stale commodity and yield data produces incorrect macro conclusions and invalid sector/portfolio recommendations.

**Freshness checklist — VERIFY ALL BEFORE PROCEEDING:**

0. **Get real system time (MANDATORY FIRST)**
   - Run `date` in the terminal to get the exact Jakarta time
   - Record the HHMM for the macro filename and the `**Last Updated:**` header
   - Example: `date` → `Thu May 28 17:36:00 WIB 2026` → filename `2026-05-28-1736-...`, header `17:36 WIB`
   - **Never guess or invent the time.** Always use the system clock.

1. **DXY (U.S. Dollar Index)**
   - Search for current price with exact date and time
   - Record with `current_price_timestamp: YYYY-MM-DD HH:MM:SS`
   - Example: "DXY 98.87 as of 2026-05-15 06:00:00 UTC (verified)"

2. **U.S. 2Y and 10Y Treasury Yields**
   - Search for latest intraday levels (not just prior close)
   - Record both current level and intraday range if trading
   - Example: "10Y yield 4.49–4.52% intraday on 2026-05-15 (verified)"

3. **Gold Spot Price**
   - Current spot price with exact timestamp
   - Example: "Gold $4,617.87/oz as of 2026-05-15 (verified)"

4. **Silver Spot Price**
   - Current spot price with timestamp
   - Example: "Silver $84–86/oz range on 2026-05-15 (verified)"

5. **WTI Crude Oil**
   - Current price with timestamp
   - Example: "WTI $100.85/bbl as of 2026-05-15 (verified)"

6. **Brent Crude Oil**
   - Current price if relevant to geopolitical setup

7. **Latest Macro Data Releases**
   - Has CPI/PPI/NFP/jobless claims/retail sales been released TODAY?
   - If so, record the ACTUAL print vs. forecast
   - Example: "NFP May 15: 220K actual vs. 220K consensus (in-line)" or "NFP not yet released, awaiting 08:30 ET print"

8. **Recent Geopolitical News**
   - Any breaking developments in last 24 hours that affect macro outlook?
   - Strait of Hormuz status? Fed speaker comments? Treasury auction results?

**Preferred sources for verification:**
- **Yahoo Finance API (yfinance):** `GC=F` (gold), `SI=F` (silver), `CL=F` (WTI), `BZ=F` (Brent), `DX-Y.NYB` (DXY), `^TNX` (10Y yield), `^VIX` — free, no API key, programmatic
- Federal Reserve FRED database (for 2Y yield `DGS2` and macro data series)
- U.S. Treasury.gov (daily yield curve rates)
- Bureau of Labor Statistics (BLS) for employment data
- U.S. Energy Information Administration (EIA) for oil prices
- TradingView, Bloomberg, Investing.com for manual cross-check and intraday detail
- Federal Reserve press releases for policy updates

**If fresh data cannot be verified, state clearly:** "Data verification incomplete; last verified price was [X] at [timestamp]; unable to confirm current levels. Review stale."

**Do not present cached or model-memory prices as current data.**

---

### 1. Refresh the current macro snapshot

Using the verified data from Step 0, record and update:

- DXY (with verification timestamp)
- U.S. 2Y Treasury yield (with verification timestamp)
- U.S. 10Y Treasury yield (with verification timestamp)
- real yields if relevant
- gold (with verification timestamp)
- silver (with verification timestamp)
- WTI (with verification timestamp)
- Brent (with verification timestamp)
- natural gas if relevant
- VIX if relevant
- Latest macro data releases that occurred today

Use exact timestamps for all prices (part of Step 0 verification).

### 2. Review geopolitics as a market driver

Focus on developments that can materially change:

- inflation expectations
- energy supply or shipping flows
- sanctions or export restrictions
- industrial commodity availability
- recession or risk-off probabilities

Do not just list headlines. State whether the development is:

- short-lived noise
- a medium-term macro driver
- a structural regime change

### 3. Build the cross-asset interpretation

Translate the setup into a practical read for:

- DXY
- front-end yields
- long-end yields
- real yields
- gold
- silver
- oil
- broad risk sentiment

Always separate:

- what is happening now
- what would confirm the trend
- what would invalidate the trend

### 4. Review the U.S. economic calendar

Identify the next releases most likely to change the view. Prioritize:

- CPI (Consumer Price Index — BLS monthly inflation gauge; above consensus = hawkish Fed, bearish gold/silver; below consensus = dovish, bullish gold/silver)
- PCE (Personal Consumption Expenditures — Fed's *preferred* inflation measure, published by BEA; Core PCE strips food/energy; above consensus = real yields rise, DXY up, gold/silver sell; below consensus = rate cut narrative unlocked, entry gate opens)
- PPI (Producer Price Index — upstream wholesale/factory prices; leads CPI by 1–2 months; PPI above consensus = CPI/PCE likely to follow higher)
- NFP and unemployment (Non-Farm Payrolls — monthly labor report; above consensus = Fed stays restrictive, bearish PMs; below consensus = cut urgency, bullish PMs)
- retail sales (consumer spending; strong = overheating economy = hawkish; weak = demand softening = dovish arc)
- ISM Manufacturing / Services PMI (Institute for Supply Management Purchasing Managers Index — above 50 = expansion, below 50 = contraction; Manufacturing PMI drives silver more than gold due to industrial demand link)
- FOMC decisions and material Fed speeches (Federal Open Market Committee — tone shifts hawkish to dovish = direct tailwind for gold/silver miners)
- Treasury auctions if yield pressure is the main issue (weak bid-to-cover = yields spike = headwind for precious metals)

For each event, note:

- exact date
- exact time
- why it matters
- which assets are most exposed

### 5. Save a structured markdown note

Save under:

- `02_market/macroeconomy/`

Recommended filename:

- `YYYY-MM-DD-HHMM-macro-top-down-review.md`

Use:

- `05_templates/macro_top_down_review_template.md`

If volatility is elevated or a major catalyst is close, also create an hour-level follow-up note:

- `YYYY-MM-DD-HH00-macro-hourly-monitor.md`

Use:

- `05_templates/macro_hourly_monitor_template.md`

### 6. Set the next review schedule

Default next-review logic:

- if the next major U.S. release is within 1 to 3 trading days, set the review for the day before or the morning of the release
- if markets are calm and no major release is close, set a weekly macro review
- if yields, DXY, gold, silver, or oil move sharply, bring the review forward immediately
- if geopolitics escalates materially, do not wait for the scheduled review date
- if the market is in a volatile intraday regime, set the next review at the next hourly checkpoint until conditions calm down

Suggested review cadence:

- weekly baseline macro review
- event-driven review before CPI, NFP, FOMC, or another major release
- same-day review after a major surprise or geopolitical shock
- hourly follow-up during volatile sessions, especially around CPI, PPI, NFP, FOMC, Treasury auctions, or sharp cross-asset repricing

### 7. End with decision-useful output

The final note should clearly state:

- macro trend verdict
- most important upcoming date
- what would change the view
- which tracked **sectors** are most exposed (text-only; do NOT open watchlist CSV or pull individual ticker data)
- whether hourly monitoring remains necessary and the exact next checkpoint

## Primary Workflow: Analyze a Ticker

### ⚠️ STEP 0: DATA FRESHNESS VERIFICATION (MANDATORY — DO THIS FIRST)

**This step is non-negotiable. Do not skip or defer.** Financial analysis with stale data produces incorrect conclusions and poor recommendations.

**Current date context:** Always check the system date. All analysis must reference the current date explicitly.

**Freshness checklist — VERIFY ALL BEFORE PROCEEDING:**

1. **Current Stock Price — read `00_inbox/prices_latest.md` first**
   - **Step 0 always starts here:** Read `00_inbox/prices_latest.md` — it contains macro + all 60 watchlist prices with drift vs. reference and a ready-to-use `current_price_timestamp`
   - **Freshness check:** The file header has `<!-- fetch_prices_timestamp: ... -->` and `<!-- stale_after_hours: 4 -->`. If the file is older than 4 hours, tell the user to run `python3 scripts/fetch_prices.py` to refresh before proceeding
   - **To refresh:** `python3 scripts/fetch_prices.py` — rewrites the file and prints the same output
   - **Single ticker not in watchlist:** `import yfinance as yf; yf.Ticker("TICKER").fast_info["last_price"]`
   - **Indonesian stocks:** append `.JK` suffix (e.g., `BBCA.JK`)
   - **Fallback for extended-hours / OTH detail:** TradingView for premarket/post-market confirmation

2. **Latest Reported Quarter & Report Date**
   - Verify which quarter was most recently reported (Q1 2026? Q4 2025?)
   - Confirm the exact filing/earnings date from SEC or company IR
   - Example: MTDR Q1 2026 reported 2026-05-06

3. **Latest Analyst Ratings & Price Targets**
   - Search for 5 most recent analyst ratings (upgrade/downgrade in last 7 days?)
   - Fetch consensus price target and compare to current price
   - Example: "Truist upgraded MTDR to Buy on May 12 with $67 PT"

4. **Current Commodity Prices (if applicable)**
   - **Oil/Energy names:** WTI crude, natural gas, Waha basis
   - **Gold/Silver miners:** Spot gold, spot silver, USD strength
   - **Solar:** Polysilicon prices, equipment costs if relevant
   - Record current prices with exact date
   - Example: "WTI crude oil $102.21/Bbl (May 13, 2026)" ≠ "oil is around $73"

5. **Post-Earnings Events**
   - Any news, guidance changes, M&A, or material announcements since last quarter?
   - Any analyst conferences, investor days, or management changes?
   - Example: "Q1 2026 earnings released May 6; Truist upgraded May 12"

6. **Macro Indicators (if relevant to thesis)**
   - US 10Y Treasury yield
   - US Manufacturing PMI
   - DXY (US dollar strength)
   - Recession signals (employment, GDP growth expectations)
   - Record with date
   - Example: "US 10Y yield 4.4%+ as of May 12-14, 2026"

7. **Post-Analysis Update Pattern**
   - After gathering fresh data, check if conclusions in old files are now stale
   - If commodity prices have changed 10%+, thesis assumptions may be broken
   - If analyst consensus moved 5%+ in either direction, re-evaluate conviction
   - If recession signals emerge, update downside scenarios immediately

**How to verify:**
- **All prices:** Read `00_inbox/prices_latest.md`; check `fetch_prices_timestamp` in the header — if older than 4 hours, ask user to run `python3 scripts/fetch_prices.py` first
- **Single stock not in watchlist:** `yf.Ticker("TICKER").fast_info["last_price"]`; Indonesian stocks use `.JK` suffix
- **2Y yield:** FRED `DGS2` or Treasury.gov (not available via Yahoo Finance)
- **Earnings date:** Search for "TICKER Q1 2026 earnings date" or visit company IR site
- **Analyst ratings:** WebSearch "TICKER analyst ratings" + check TipRanks
- **News:** Check company press releases, 8-K filings, and financial news for last 30 days

**Example of what freshness failure looks like:**
❌ WRONG: "Oil is around $73/Bbl (from Q1 2026 analysis)" → Later discover WTI at $102 (40% difference!)
❌ WRONG: "Latest quarter Q1 2026" → Don't verify if Q2 was already reported
❌ WRONG: "Analyst consensus $61" → Miss that Truist just upgraded to $67 yesterday
✅ RIGHT: "Current price $57.31 (May 13, 2026 close), WTI $102.21 (May 13), analyst consensus ~$63-67 post-Truist upgrade (May 12), latest quarter Q1 2026 (reported May 6)"

**If you skip this step:** You will present outdated financial conclusions, miss material catalysts, and the analysis will be stale within days. The MTDR case study showed oil at $73 was 40% wrong vs. current $102.

---

### 1.5. Read Sector-Specific Template and Checklist (MANDATORY)

**Before any analysis work, you MUST read the sector template and checklist.** These define what sections, metrics, and depth of analysis are required for that sector.

**Workflow:**
1. Identify the company's sector (gold/silver miners, oil/energy, technology, automotive, solar)
2. Read the matching template from `05_templates/[SECTOR]_company_template.md`
3. Read the matching checklist from `05_templates/[SECTOR]_research_checklist.md`
4. Read the matching earnings comparison template from `05_templates/earnings_comparison_template.md`

**Templates available:**
- `05_templates/gold_miners_company_template.md` + `gold_miners_research_checklist.md`
- `05_templates/silver_miners_company_template.md` + `silver_miners_research_checklist.md`
- `05_templates/oil_energy_upstream_ep_company_template.md` + `oil_energy_upstream_ep_research_checklist.md`
- `05_templates/ai_compute_infrastructure_company_template.md` + `ai_compute_infrastructure_research_checklist.md`
- `05_templates/power_cooling_infrastructure_company_template.md` + `power_cooling_infrastructure_research_checklist.md`
- `05_templates/power_generation_nuclear_company_template.md` + `power_generation_nuclear_research_checklist.md`
- `05_templates/automotive_company_template.md` + `automotive_research_checklist.md`
- `05_templates/solar_energy_company_template.md` + `solar_energy_research_checklist.md`
- `05_templates/saas_company_template.md` + `saas_research_checklist.md`
- `05_templates/banking_company_template.md` + `banking_research_checklist.md`
- Generic fallback: `05_templates/company_analysis_template.md` + `05_templates/earnings_comparison_template.md`

**Why:** The template defines the required structure (sections, key metrics, depth) for a complete analysis in that sector. Skipping this step results in incomplete or inconsistently structured notes.

---

### 2. Identify the company and sector

Search for an existing company note:

- `03_sectors/[sector]/companies/TICKER-*.md`
- `04_portfolio/watchlist/watchlist.csv`

Current sector folders in use:

- `03_sectors/gold_miners/`
- `03_sectors/silver_miners/`
- `03_sectors/oil_energy/`
- `03_sectors/technology/`
- `03_sectors/automotive/`
- `03_sectors/solar_energy/`
- `03_sectors/power_infrastructure/`
- `03_sectors/power_generation/`
- `03_sectors/saas/`
- `03_sectors/banking/`

If the company does not exist yet:

- infer the most likely sector
- create the company note in the matching `companies/` folder
- use the correct sector template from `05_templates/` (which you've already read in Step 1.5)

Before starting the analysis, check whether the ticker already exists in the watchlist.

If it exists, use these watchlist fields as the default market reference:

- `exchange`
- `reference_price`
- `last_analyzed_at`

Use them to:

- identify the primary listed exchange to analyze
- anchor the starting price context in the company note and earnings comparison
- compare current research conclusions against the last tracked watchlist price

Use the primary listed exchange to keep currency formatting consistent:

- `NYSE` / `NASDAQ` names: use `USD ($)` for current price, entry zones, price targets, and short-term price scenarios
- If the company reports earnings in another currency, keep reported financials and guidance in that local currency, but add `USD` alongside when useful for readability and consistency

If the watchlist has no entry yet:

- create one by the end of the analysis
- populate `current_price`, `current_price_timestamp`, `reference_price`, and `last_analyzed_at`

---

### 3. Use Step 0 freshness data to determine the reporting quarter set

**You have already verified the latest quarter in Step 0.** Now use that information to build the quarter set.

For earnings-driven analysis, always gather these three periods:

- Latest reported quarter (verified in Step 0)
- Immediately previous quarter
- Same quarter one year earlier

Examples:

- If latest is `Q1 2026`, gather `Q1 2026`, `Q4 2025`, and `Q1 2025`
- If latest is `Q3 2026`, gather `Q3 2026`, `Q2 2026`, and `Q3 2025`

### 4. Download and save earnings materials (was Step 4)

Save source materials under:

- `03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]/`

Check TradingView `Documents` first.

Prefer official company sources in this order:

1. TradingView `Documents` page for the quarter's earnings release, presentation, and filing links
2. Official earnings release
3. Earnings presentation
4. Shareholder letter
5. 10-Q / 10-K / 6-K if useful
6. Earnings call transcript or conference call page

For each quarter folder, include a `README.md` with:

- Company name
- Quarter
- Earnings release date
- Saved local files
- Original source URLs
- Notes on missing documents

### 5. Build the multi-quarter comparison note (was Step 5)

Create or update:

- `03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]-comparison.md`

Use:

- `05_templates/earnings_comparison_template.md`

This file should compare all three quarters side-by-side and must include:

- Executive summary
- Multi-quarter metrics table
- QoQ analysis
- YoY analysis
- Trend interpretation
- Latest management guidance
- Changes versus prior guidance when available
- Investment implications
- Pre-earnings prediction when results have not been reported yet
- Source log
- Current timeline header with:
  - `analysis_date`
  - `latest_reported_quarter`
  - `latest_report_date`
  - `current_price`
  - `current_price_timestamp`
  - `post_earnings_events`

For China EV automotive names, also add a monthly delivery trend section using `https://cnevpost.com/` when available. Include:

- last 3 to 12 months of delivery data depending on availability
- model-level sales breakdown when available
- YoY and MoM trend interpretation
- whether volume growth is broad-based or concentrated in one model

For China EV automotive names, also check `https://carnewschina.com/` for:

- new model launches
- facelifts and refresh cycles
- price cuts or pricing changes
- trim, battery, range, or spec updates
- launch timing that may explain monthly delivery inflections

If the next earnings report has not been released yet, add a pre-earnings prediction section to the comparison note. Include:

- expected beat / inline / miss view
- expected revenue, margin, delivery, and guidance direction
- short-term post-earnings price scenario
- the price range that fits the prediction over the next few days to two weeks
- the reason for that price view, tied to valuation, positioning, delivery trend, and product-cycle evidence
- what result would invalidate the prediction

### 6. Capture latest guidance explicitly (was Step 6)

The latest earnings report must have a dedicated guidance section in the comparison note.

Always extract, when available:

- Revenue guidance
- EPS guidance
- Production or delivery guidance
- Cost guidance
- Margin guidance
- Capex guidance
- Free cash flow or balance sheet targets
- Project timing or ramp expectations
- Management commentary on next quarter / second half / full year

Also state whether guidance was:

- Raised
- Maintained
- Lowered
- Newly introduced
- Not provided

### 7. Create or update the company note (was Step 7)

Create or update:

- `03_sectors/[sector]/companies/TICKER-company-name.md`

Use the sector-specific company template when available:

- `05_templates/gold_miners_company_template.md`
- `05_templates/silver_miners_company_template.md`
- `05_templates/oil_energy_upstream_ep_company_template.md`
- `05_templates/ai_compute_infrastructure_company_template.md`
- `05_templates/power_cooling_infrastructure_company_template.md`
- `05_templates/power_generation_nuclear_company_template.md`
- `05_templates/automotive_company_template.md`
- `05_templates/solar_energy_company_template.md`
- `05_templates/saas_company_template.md`
- `05_templates/banking_company_template.md`

Fallback:

- `05_templates/company_analysis_template.md`

The company note should synthesize, not just restate, the earnings comparison:

- current thesis
- key catalysts
- risks
- financial quality
- valuation framing
- what to monitor
- decision and next review date

Currency formatting rule for company notes:

- Stock-price references should follow the primary listing currency
- For U.S.-listed names, stock-price language should stay in `USD ($)`
- Reported financials, guidance, and balance-sheet figures may remain in the company's reporting currency, with `USD` shown alongside when helpful

The top section of the company note should also include a brief `Current Context` block covering:

- latest verified price and `current_price_timestamp`
- latest reported quarter and report date
- next earnings date if known
- key post-earnings development since the latest quarter
- whether the thesis is improving, stable, or deteriorating right now

### 8. Update watchlist if appropriate (was Step 8)

If the company is actively being monitored or the user asked for a fresh analysis, update:

- `04_portfolio/watchlist/watchlist.csv`

Focus on:

- `current_price`
- `current_price_timestamp`
- `reference_price`
- `last_analyzed_at`
- `status`
- `thesis_summary`
- `next_review_scheduled_at`
- `exchange`

Watchlist schema:

- `ticker`
- `company`
- `status`
- `current_price`
- `current_price_timestamp`
- `target_buy_zone`
- `entry_idea`
- `theme`
- `thesis_summary`
- `reference_price`
- `last_analyzed_at`
- `next_review_scheduled_at`
- `exchange`
- `sector`

Field intent:

- `current_price` = latest refreshed market price for watchlist maintenance
- `current_price_timestamp` = exact timestamp for that market-price refresh, using Jakarta local time with hour, minute, and second
- `reference_price` = anchor price captured during the most recent full analysis
- `last_analyzed_at` = date the most recent `full TICKER` workflow was completed

If the user explicitly asked to `update watchlist`, do this across the active watchlist, not just for the current ticker.

For the `update watchlist` command:

- refresh `current_price` and `current_price_timestamp` for each active row
- record `current_price_timestamp` with full time precision, not just date
- store it in Jakarta local time without a timezone suffix
- leave `reference_price` and `last_analyzed_at` unchanged unless a fresh full analysis is also completed
- keep `exchange` unchanged unless there is a verified listing-context reason to change it
- compare the refreshed price against `target_buy_zone`
- flag rows where the current price is:
  - inside the target buy zone
  - within roughly 5% to 10% of the target zone and worth re-checking soon
  - far enough away that no action is needed
- if a company has a stale thesis because of fresh earnings, guidance, financing, or macro changes, note that it should be re-analyzed before entry even if price is attractive
- update `next_review_scheduled_at` when the catalyst calendar has clearly changed

### 9. Set next review timing (was Step 9)

Default next review date:

- 5 to 10 business days before the next expected earnings release

Bring the review date forward if there is an important catalyst before earnings, such as:

- guidance update
- commodity move
- product launch
- mine ramp milestone
- regulatory event

## Pulse Ticker Workflow

Use this workflow when the user says `pulse TICKER`.

This is a **lightweight exogenous refresh only**. Do not rebuild fundamentals, valuation, or earnings comparisons. Do not re-run sector templates or research checklists.

### Step 0: Confirm current date and locate the company file

- Confirm today's date
- Locate the existing company file: `03_sectors/[sector]/companies/TICKER-*.md`
- Read the watchlist row to pull `reference_price` and `last_analyzed_at`

### Step 1: Price drift check

- Get the current verified price
- Compare against `reference_price` in watchlist CSV
- Calculate % change and note the direction since the last full analysis

### Step 2: Recent headlines (last 7–14 days)

Search for:
- Press releases and 8-K filings
- Management commentary at conferences or investor days
- News articles or analyst notes that reference material developments
- Anything that could shift the thesis or change entry timing

Focus on events since `last_analyzed_at`.

### Step 3: Sector macro backdrop (2–3 indicators only)

Pull only the indicators that directly drive the sector. Do not run a full macro review.

| Sector | Indicators to check |
|--------|---------------------|
| Gold miners | Real yields + DXY + gold spot |
| Silver miners | ISM PMI + copper + silver spot |
| Oil / energy | WTI + EIA inventory + rig count |
| Banking (US) | Fed funds rate + 2Y/10Y yields |
| Banking (Indonesia) | BI Rate + USD/IDR + JCI |
| Technology / AI | NVDA sector sentiment + macro risk-on/off |
| Solar | Polysilicon price + IRA policy news |
| Automotive | Monthly delivery data + incentive news |

### Step 4: Analyst rating or price target changes

- Check TipRanks or search for any rating or price target changes since `last_analyzed_at`
- Note: firm name, date, old vs. new rating, old vs. new price target
- If no changes, state "None since last analysis"

### Step 5: Corporate actions

Check for any of the following since the last full analysis:
- Secondary equity offerings
- Buyback authorizations or completions
- Material insider purchases or sales
- Dividend changes
- Guidance revisions (pre-announcement or formal update)
- M&A announcements: acquisition, merger, spinoff, asset sale

### Step 6: Update company file and append changelog block

**Sections to update** in `03_sectors/[sector]/companies/TICKER-*.md`:

| Section | What to update |
|---------|---------------|
| Snapshot | `current_price` and `current_price_timestamp` only |
| Current Context | Latest price, recent headlines, post-analysis events |
| Thesis | Update bull/base/bear narrative if macro or geopolitical shifts change the story |
| Why This Could Work | Add or revise catalysts if new ones emerge from headlines |
| Key Risks | Add, remove, or reprioritize risks based on new developments |
| What To Monitor | Revise watchpoints if new headlines change what matters next |
| Decision | Conviction level, entry zone timing, next review date |

**Sections to leave unchanged** (requires financial recomputation — do not edit):
Business Summary, Financial Quality, Valuation

After updating the sections above, **append this changelog block at the bottom** of the file:

```markdown
## Pulse Update – YYYY-MM-DD

**Price drift:** $XX.XX → $XX.XX (+X.X% since YYYY-MM-DD analysis)
**Macro backdrop:** [2–3 sector-relevant indicators and direction]
**Recent headlines:**
- YYYY-MM-DD: [headline or event summary]
- YYYY-MM-DD: [headline or event summary]
**Analyst changes:** [changes since last_analyzed_at, or "None since last analysis"]
**Corporate actions:** [offerings, buybacks, guidance revisions, 8-K, or "None"]
**Thesis status:** Intact / Watch / Challenged
**Sections updated:** [e.g., "Current Context, Decision"]
**Trigger full re-analysis?** Yes / No — [one-line reason]
**Next pulse:** YYYY-MM-DD
```

### Step 7: Update watchlist CSV (two fields only)

Update **only** these two fields in `04_portfolio/watchlist/watchlist.csv`:
- `current_price` — latest verified price
- `current_price_timestamp` — Jakarta local time `YYYY-MM-DD HH:MM:SS`

Do **not** update `reference_price` or `last_analyzed_at`. Those stay anchored to the last `full TICKER` run.

---

## Sector Routing Rules

Use the company’s operating model to choose the template.

### Gold Miners

Use:

- `05_templates/gold_miners_company_template.md`
- `05_templates/gold_miners_research_checklist.md`

### Silver Miners

Use:

- `05_templates/silver_miners_company_template.md`
- `05_templates/silver_miners_research_checklist.md`

Focus on:

- production
- grade
- AISC / cash costs
- reserve life
- jurisdiction risk
- mine ramp and project pipeline
- metal price leverage

### Oil and Energy

Use:

- `05_templates/oil_energy_upstream_ep_company_template.md`
- `05_templates/oil_energy_upstream_ep_research_checklist.md`

Focus on:

- oil, gas, and NGL production
- realized pricing
- capital efficiency
- inventory depth
- well productivity
- balance sheet and shareholder returns
- basin differentials and transport constraints

### Technology

Use:

- `05_templates/technology_company_template.md`
- `05_templates/technology_research_checklist.md`

Focus on:

- revenue growth quality
- gross margin and operating leverage
- recurring revenue
- product mix
- customer concentration
- AI / cloud / semiconductor cycle where relevant
- guidance credibility

### Automotive

Use:

- `05_templates/automotive_company_template.md`
- `05_templates/automotive_research_checklist.md`

Focus on:

- unit volumes
- ASP
- gross margin
- inventory
- dealer demand
- EV mix
- incentive intensity
- supply chain and tariff exposure

For China EV names, also check `https://cnevpost.com/` for monthly delivery releases and model-level sales breakdowns to compare trend direction versus the quarterly earnings story.
Also check `https://carnewschina.com/` for new model release cadence and product updates that may explain changes in deliveries, mix, and demand.
If earnings are upcoming, form a pre-earnings view and a short-term price reaction scenario based on monthly sales momentum, model mix, and launch cadence rather than waiting for the report.

### Solar Energy

Use:

- `05_templates/solar_energy_company_template.md`
- `05_templates/solar_energy_research_checklist.md`

Focus on:

- shipments / MW deployed or sold
- ASP and mix
- gross margin and manufacturing yield
- capacity, utilization, and ramp execution
- backlog / project pipeline / bookings
- IRA, domestic content, and tariff exposure
- liquidity and funding needs

### SaaS

Use:

- `05_templates/saas_company_template.md`
- `05_templates/saas_research_checklist.md`
- `05_templates/saas_earnings_comparison_template.md` (preferred over generic earnings comparison template)

Focus on:

- ARR (Annual Recurring Revenue) and NRR (Net Revenue Retention)
- subscription revenue growth vs. professional services mix
- gross margin and operating leverage trajectory
- Rule of 40 score (revenue growth % + FCF margin %)
- customer count, ACV, and land-and-expand metrics
- AI monetization: copilot attach rate, agent seats, usage-based ARR
- guidance credibility and beat-and-raise cadence
- FCF conversion and margin inflection timing

### Banking

Use:

- `05_templates/banking_company_template.md`
- `05_templates/banking_research_checklist.md`

Applies to both U.S. banks (JPM, BAC, WFC, C, USB, PNC) and Indonesian banks (BBCA, BBRI, BMRI, BBNI, BTN).

Focus on:

- **NIM (Net Interest Margin)**: Primary revenue driver; rate-cycle sensitive
- **NII (Net Interest Income)**: NIM × earning assets; must track QoQ and YoY trend
- **NPL / NCO**: Non-performing loans and net charge-offs; credit quality signal
- **Provision expense**: PCL (Provision for Credit Losses) trend vs. net charge-off coverage
- **ROTCE / ROE**: Quality-of-earnings benchmark; 15–20%+ target for U.S. banks; 18–22%+ for Indonesian top-tier
- **CET1 / CAR**: Capital adequacy; buffer for buybacks and dividends (U.S.: CET1; Indonesia: CAR)
- **CASA ratio** (Indonesian banks): Funding cost proxy; higher CASA = lower cost of funds
- **BOPO** (Indonesian banks): Cost-to-income efficiency ratio; target <45% for BBCA
- **LDR** (Indonesian banks): Loan-to-deposit ratio; lending capacity signal
- **Loan growth by segment**: Corporate, retail/consumer (KPR/KKB/personal), SME/UMKM
- **Fee income / non-interest income**: Wealth management, cards, trade finance, IB fees
- **Capital return**: Buyback authorization, dividend yield, payout ratio
- **Regulatory context**: U.S. DFAST/CCAR; Indonesia OJK GWM, POJK capital rules; rate environment from Fed/BI

Key macro drivers to monitor for banking:
- Fed Funds / BI rate path: rate hikes = NIM expansion; rate cuts = NIM compression
- Yield curve shape: steep = structural NIM tailwind; flat/inverted = headwind
- Credit cycle: NPL formation trend is a leading indicator of provision pressure
- Indonesian GDP and consumption growth: drives retail loan and UMKM demand

## Output Standards

### Earnings comparison file

This is the operational truth file for the quarter and should be rich in direct evidence.

Minimum sections:

1. Executive Summary
2. Current Timeline
3. Quarter Set and Source Log
4. Multi-Quarter Comparison Table
5. Quarter-over-Quarter Analysis
6. Year-over-Year Analysis
7. Guidance and Management Commentary
8. Trend Verdict
9. Investment Implications
10. Next Review Trigger

### Company analysis file

This is the durable investment memo and should be readable on its own.

Minimum sections:

1. **Snapshot** ← Must include:
   - Current price and `current_price_timestamp`
   - **Bear case:** Price target + short description (e.g., "Margin compression, commodity downturn") + upside/downside % from current price
   - **Base case:** Price target + short description (e.g., "In-line production, stable guidance") + upside/downside % from current price
   - **Bull case:** Price target + short description (e.g., "Production beat, cost outperformance") + upside/downside % from current price
   - Quick snapshot of sector, market cap, exchange
2. Current Context
3. Business Summary
4. Thesis
5. Why This Could Work
6. Key Risks
7. Financial Quality
8. Valuation
9. What To Monitor
10. Decision
11. Related Documents

### Macro screenshot analysis file

This is the repo note for pasted market or calendar screenshots and should turn image data into a usable trading interpretation.

Minimum sections:

1. Screenshot Summary
2. Event Table
3. Surprise Assessment
4. Impact on Gold
5. Impact on Silver
6. Impact on DXY
7. Impact on 2Y and 10Y Yields
8. Cross-Asset Trend Read
9. What To Watch Next
10. Related Sectors or Tickers

## Source Quality Rules

Prefer primary sources first.

Highest priority:

1. Company investor relations site
2. SEC filings or equivalent official filings
3. Official earnings presentation or shareholder letter

Secondary sources are acceptable for context only:

- finance portals
- analyst summaries
- market data pages

Do not rely on secondary sources when the primary document is available.

For fast-moving financial facts, stale notes inside the repository are also secondary context. They should be updated after verification, not treated as proof of current reality.

## Naming Conventions

### Company notes

- `03_sectors/[sector]/companies/TICKER-company-name.md`

### Earnings folders

- `03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]/`

### Comparison notes

- `03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]-comparison.md`

## If the user says only "analyze TICKER" or "full TICKER"

Do all of the following by default, **IN THIS EXACT SEQUENCE:**

**0. STEP 0: DATA FRESHNESS VERIFICATION (MANDATORY FIRST)**
   - Verify current date, latest reported quarter, current stock price, commodity prices, analyst ratings, post-earnings events, macro indicators
   - Search for fresh data; do NOT rely on cached model knowledge
   - Record all prices/dates with exact timestamps

**1.5. STEP 1.5: READ SECTOR TEMPLATE & CHECKLIST (MANDATORY BEFORE ANALYSIS)**
   - Identify the sector (gold/silver miners, oil/energy, technology, automotive, solar, saas, banking)
   - Read the sector-specific company template from `05_templates/`
   - Read the sector-specific research checklist from `05_templates/`
   - Read the earnings comparison template from `05_templates/`
   - **This step ensures you understand what a complete analysis looks like for that sector**

1. Identify the company and sector (find existing company file and watchlist data)
2. Determine the reporting quarter set using freshness data from Step 0
3. Download the latest quarter, previous quarter, and same quarter last year materials
4. Save files under the correct earnings folder
5. Create or update the multi-quarter comparison note using sector template structure
6. Create or update the company analysis note using sector-specific template
7. Update the watchlist entry if appropriate (use Step 0 current price and dates)
8. Set the next review date

## Templates to Use

- `05_templates/company_analysis_template.md`
- `05_templates/earnings_comparison_template.md`
- `05_templates/gold_miners_company_template.md`
- `05_templates/gold_miners_research_checklist.md`
- `05_templates/silver_miners_company_template.md`
- `05_templates/silver_miners_research_checklist.md`
- `05_templates/oil_energy_upstream_ep_company_template.md`
- `05_templates/oil_energy_upstream_ep_research_checklist.md`
- `05_templates/ai_compute_infrastructure_company_template.md`
- `05_templates/ai_compute_infrastructure_research_checklist.md`
- `05_templates/power_cooling_infrastructure_company_template.md`
- `05_templates/power_cooling_infrastructure_research_checklist.md`
- `05_templates/power_generation_nuclear_company_template.md`
- `05_templates/power_generation_nuclear_research_checklist.md`
- `05_templates/automotive_company_template.md`
- `05_templates/automotive_research_checklist.md`
- `05_templates/solar_energy_company_template.md`
- `05_templates/solar_energy_research_checklist.md`
- `05_templates/saas_company_template.md`
- `05_templates/saas_research_checklist.md`
- `05_templates/saas_earnings_comparison_template.md`
- `05_templates/banking_company_template.md`
- `05_templates/banking_research_checklist.md`

## Practical Example

If the user says `analyze EXK`, the agent should:

**0. STEP 0:** Verify current date, EXK stock price, Q1 2026 latest quarter, analyst ratings, silver/gold prices
**1.5. STEP 1.5:** Read `silver_miners_company_template.md`, `silver_miners_research_checklist.md`, and `earnings_comparison_template.md` to understand required sections and metrics
1. Locate `03_sectors/silver_miners/companies/EXK-endeavour-silver.md` and watchlist entry
2. Confirm the latest reported quarter is Q1 2026 (reported May 6-7, 2026)
3. Save the latest report, prior quarter report, and same quarter prior year report under `03_sectors/silver_miners/earnings/EXK/`
4. Create or refresh `03_sectors/silver_miners/earnings/EXK/2026-Q1-comparison.md` using the earnings template
5. Extract the latest guidance (production, AISC, capex) and compare across three quarters
6. Update the EXK company memo using the silver miners template (Snapshot, Current Context, Thesis, Why This Could Work, Key Risks, Operating Quality, Financial Quality, Valuation, What To Monitor, Decision, Related Documents)
7. Update watchlist status, analyst consensus, conviction, and next review date if needed
8. Ensure all prices tagged with `current_price_timestamp` and use exact dates throughout

If the user says `full EXK`, the agent should follow the same sequence, but with stricter completion requirements:

- do not stop after a quick summary
- do not skip the sector checklist
- do not skip the earnings comparison note
- do not skip the company note refresh
- do not skip watchlist updates when relevant

## Timestamp Format Rules

**CSV Files (watchlist, holdings, transactions, etc.):**
- Use format: `YYYY-MM-DD HH:MM:SS` (e.g., `2026-05-15 08:00:00`)
- Store timestamps in **Jakarta local time**
- Do **NOT** append timezone label or "Jakarta time" inside the CSV
- Include hour, minute, and second for full precision
- Example CSV entry: `current_price_timestamp: 2026-05-15 08:00:00` (no timezone suffix)

**Markdown Files (analysis notes, macro notes, etc.):**
- File names may include timestamps: `YYYY-MM-DD-HHMM-macro-top-down-review.md`
- Headers MAY include "Jakarta time" for clarity: `**Last Updated:** 2026-05-15 08:00 Jakarta time`
- This helps readers understand timezone context without cluttering the CSV data

**Rule:** Timestamps in CSV data should be clean (YYYY-MM-DD HH:MM:SS, no suffix), while markdown file headers can include "Jakarta time" for human readability.
