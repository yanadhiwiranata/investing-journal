# Workflow Checklist for Ticker Analysis

**Use this checklist before analyzing any ticker.** Reference the appropriate files below to ensure nothing is missed.

## Pre-Analysis (Required Every Time)

- [ ] **Step 0: Data Freshness** (See AGENT.md Step 0)
  - [ ] Verify current date (today is 2026-05-14)
  - [ ] Search for current stock price with exact date
  - [ ] Confirm latest reported quarter from SEC/IR
  - [ ] Search for latest analyst ratings (TipRanks, 5 most recent)
  - [ ] Verify current commodity prices if applicable
  - [ ] Check for post-earnings events (guidance changes, M&A, etc.)
  - [ ] Check for macro shifts relevant to thesis

- [ ] **Step 1.5: Read Sector Template & Checklist** (See AGENT.md Step 1.5 — MANDATORY)
  - [ ] Identify the sector (gold/silver miners, oil/energy, technology, automotive, solar)
  - [ ] Read `05_templates/[SECTOR]_company_template.md` to understand required sections
  - [ ] Read `05_templates/[SECTOR]_research_checklist.md` to understand key metrics
  - [ ] Read `05_templates/earnings_comparison_template.md` for earnings structure
  - [ ] **Critical:** This defines what a COMPLETE analysis looks like for this sector

- [ ] **Step 1: Identify Company & Sector** (See CLAUDE.md "Working with Tickers")
  - [ ] Find company file: `03_sectors/[sector]/companies/TICKER-*.md`
  - [ ] Check watchlist row: `04_portfolio/watchlist/watchlist.csv`
  - [ ] Identify primary exchange and reference price

## Analysis & Output (Follow Template Structure)

- [ ] **Step 2: Gather Earnings Data** (See AGENT.md Steps 3–4)
  - [ ] Identify latest 3 quarters (current + prior + YoY)
  - [ ] Verify earnings dates and report status
  - [ ] Note if next earnings is already scheduled or upcoming

- [ ] **Step 3: Create/Update Earnings Comparison** (See AGENT.md Step 5)
  - [ ] Use template: `05_templates/earnings_comparison_template.md`
  - [ ] File location: `03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]-comparison.md`
  - [ ] **REQUIRED SECTIONS:**
    - Executive Summary
    - Current Timeline (with analysis_date, latest_reported_quarter, latest_report_date, current_price, current_price_timestamp)
    - Multi-Quarter Comparison Table (with QoQ and YoY trends)
    - Analyst Ratings & Street Consensus (5 most recent, consensus PT, upside/downside %)
    - Guidance & Management Commentary
    - Trend Verdict
    - Investment Implications
    - Next Review Trigger

- [ ] **Step 4: Create/Update Company File** (See AGENT.md Step 7)
  - [ ] Use sector template: See list below
  - [ ] File location: `03_sectors/[sector]/companies/TICKER-company-name.md`
  - [ ] **REQUIRED SECTIONS:**
    - Snapshot (with current price, current_price_timestamp)
    - Current Context (timeline, latest quarter, next earnings, post-earnings events)
    - Business Summary
    - Thesis (Bull/Base/Bear cases)
    - Why This Could Work
    - Key Risks
    - Financial Quality
    - Valuation (methods, comparables, price targets with scenarios)
    - What To Monitor (quarterly metrics, earnings dates, milestones, metal prices)
    - Decision (Rating, Conviction, Entry Zones, Invalidation Signs, Next Review)
    - Related Documents

- [ ] **Step 5: Update Watchlist** (See AGENT.md Step 8)
  - [ ] File: `04_portfolio/watchlist/watchlist.csv`
  - [ ] Update: `current_price`, `current_price_timestamp`, `reference_price`, `last_analyzed_at`, `status`, `thesis_summary`, `next_review_scheduled_at`
  - [ ] Thesis summary should reflect **trend direction** (momentum, guidance, catalyst timing)

- [ ] **Step 6: Set Next Review Date** (See AGENT.md Step 9)
  - [ ] Default: 5–10 business days before next earnings
  - [ ] Advance if: major catalyst, commodity shock, geopolitical event, guidance change

## Sector Templates to Use

| Sector | Template File | Checklist |
|--------|---------------|-----------|
| **Gold & Silver Miners** | `gold_silver_miners_company_template.md` | `gold_silver_miners_research_checklist.md` |
| **Oil & Energy** | `oil_energy_upstream_ep_company_template.md` | `oil_energy_upstream_ep_research_checklist.md` |
| **Technology** | `technology_company_template.md` | `technology_research_checklist.md` |
| **Automotive** | `automotive_company_template.md` | `automotive_research_checklist.md` |
| **Solar Energy** | `solar_energy_company_template.md` | `solar_energy_research_checklist.md` |

## Generic Template (Fallback)

If no sector template exists: `company_analysis_template.md`

## Key Rules

1. **MANDATORY SEQUENCE:** Read WORKFLOW-CHECKLIST → AGENT.md → IDENTIFY SECTOR → READ SECTOR TEMPLATE/CHECKLIST → ANALYZE
   - Skipping the sector template reading (Step 1.5) results in incomplete analysis
   - The template defines what sections and metrics are required for that sector
2. **Always read AGENT.md first** for ticker-analysis requests (see CLAUDE.md Quick Start)
3. **Read the sector template AND checklist BEFORE analyzing** (Step 1.5)
   - Gold/silver miners: `gold_silver_miners_company_template.md` + `gold_silver_miners_research_checklist.md`
   - Oil/energy: `oil_energy_upstream_ep_company_template.md` + checklist
   - Technology: `technology_company_template.md` + checklist
   - Automotive: `automotive_company_template.md` + checklist
   - Solar: `solar_energy_company_template.md` + checklist
4. **Use the earnings comparison template** as the single source of truth for multi-quarter analysis
5. **Use sector-specific company template** — never use generic template if sector template exists
6. **Include analyst consensus** in every earnings comparison file (TipRanks, 5 most recent ratings)
7. **Use exact dates** in all notes (e.g., `2026-05-14`, not "last week")
8. **Tag current price with `current_price_timestamp`** in every file using full time precision (`YYYY-MM-DD HH:MM:SS`)
9. **Update watchlist after analysis** to reflect thesis, conviction, and next review date

## Quick Reference: File Locations

```
03_sectors/[sector]/companies/TICKER-company-name.md          ← Company analysis
03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]-comparison.md ← Earnings comparison
03_sectors/[sector]/earnings/[TICKER]/[YYYY-QN]-[TICKER].md   ← Individual quarter (optional)
04_portfolio/watchlist/watchlist.csv                          ← Watchlist tracking
05_templates/earnings_comparison_template.md                  ← Use for earnings files
05_templates/[SECTOR]_company_template.md                     ← Use for company files
05_templates/[SECTOR]_research_checklist.md                   ← Reference for key metrics
```

## Example: Analyze CDE (Gold Miner)

1. ✅ Read AGENT.md Step 0 (freshness verification)
2. ✅ Search: current price, latest quarter, analyst ratings, commodity prices
3. ✅ Use template: `gold_silver_miners_company_template.md`
4. ✅ Create/update: `03_sectors/gold_silver_miners/earnings/CDE/2026-Q1-comparison.md`
   - Include current analyst consensus in structured table
5. ✅ Update: `03_sectors/gold_silver_miners/companies/cde-coeur-mining.md`
   - Use template sections (Snapshot, Current Context, Thesis, Valuation, Decision, etc.)
6. ✅ Refresh: `04_portfolio/watchlist/watchlist.csv`
   - Update price, conviction, thesis summary, next review date

## Additional Command: Update Watchlist

Use this when the user says `update watchlist`.

- [ ] Open `04_portfolio/watchlist/watchlist.csv`
- [ ] Refresh `current_price` and `current_price_timestamp` for active watchlist rows
- [ ] Record `current_price_timestamp` with hour, minute, and second
- [ ] Use Jakarta local time in the watchlist and do not append the timezone suffix
- [ ] Compare each refreshed price against `target_buy_zone`
- [ ] Flag names already in entry range
- [ ] Flag names near entry range that may need a re-analysis
- [ ] Check whether any thesis is stale after fresh earnings, guidance, financing, or macro changes
- [ ] Update `next_review_scheduled_at` if catalyst timing changed
- [ ] Return a concise summary of which names are actionable now versus which need deeper work first

## Additional Command: Update Today

Use this when the user says `update today` or asks for the full daily maintenance pass.

- [ ] Run `update macro` first
- [ ] Run `update watchlist` second
- [ ] Run `update porto` third
- [ ] Keep the sequence intact so macro context informs the watchlist and portfolio updates
- [ ] Return one concise end-of-run summary covering macro changes, watchlist actions, and portfolio record changes

## Additional Command: Update Macro

Use this when the user says `update macro`, asks for a top-down market refresh, or wants a review of geopolitics, DXY, yields, and commodities.

- [ ] Treat `update macro` as a higher-priority macro refresh, not a light periodic check
- [ ] Review geopolitics that could affect inflation, growth, energy, shipping, sanctions, or supply chains
- [ ] Refresh: DXY, U.S. 2Y yield, U.S. 10Y yield, and real yields if relevant
- [ ] Refresh key commodities relevant to the portfolio:
  - [ ] Gold
  - [ ] Silver
  - [ ] WTI / Brent
  - [ ] Other focus commodities if relevant
- [ ] Identify whether the setup is:
  - [ ] Dollar bullish / neutral / bearish
  - [ ] Yield bullish / neutral / bearish
  - [ ] Commodity supportive / neutral / hostile
- [ ] Review the next important U.S. economic releases:
  - [ ] CPI
  - [ ] PPI
  - [ ] NFP / unemployment
  - [ ] Retail sales
  - [ ] ISM PMI
  - [ ] FOMC / Fed speakers if material
  - [ ] Treasury auctions if yields are the main driver
- [ ] Save a structured top-down note in `02_market/macroeconomy/`
- [ ] Decide whether an hourly note is required:
  - [ ] Use hourly notes if the data is moving quickly, a major release is due within the same session, or DXY/yields/gold/silver/oil are repricing sharply
  - [ ] Save hourly notes in `02_market/macroeconomy/` using an hour-level filename
- [ ] Set the next review time based on the nearest meaningful catalyst, not just a fixed cadence
- [ ] Use hourly follow-up timing during volatile conditions instead of waiting for the next daily or weekly review
- [ ] Bring the next review forward immediately if there is a geopolitical escalation or a large move in DXY, yields, gold, silver, or oil

## Additional Command: Update Porto

Use this when the user says `update porto` or asks to update portfolio records.

- [ ] Update `04_portfolio/holdings/holdings_snapshot.csv` for live positions only
  - [ ] **Do NOT include tickers with 0 shares** — holdings should only list active positions
  - [ ] Include `sector` column
- [ ] Refresh holdings metrics such as current price and unrealized P/L
- [ ] Update `04_portfolio/transactions/open_orders.csv` for open pending orders
  - [ ] Include `sector` column
  - [ ] **Sort by: sector (asc), ticker (asc), action (asc), limit_price (asc)**
- [ ] Update `04_portfolio/transactions/transactions.csv` for completed fills
  - [ ] Include `sector` column
  - [ ] **Sort by: sector (asc), ticker (asc), action (asc), price (asc)**
- [ ] Keep open orders separate from completed transactions
- [ ] Use exact dates in all portfolio files

---

## ⚠️ CRITICAL REMINDERS

- **READ THE SECTOR TEMPLATE FIRST (Step 1.5).** This is not optional. The template defines the required structure and metrics for that sector.
- **If you skip the template reading, your analysis will be incomplete.** You may miss required sections (e.g., Operating Quality, Guidance, Analyst Consensus).
- **The template is the specification.** If the template requires 10 sections and the company file has 8, your update is incomplete.

---

**Always complete this checklist before claiming analysis is done.**
