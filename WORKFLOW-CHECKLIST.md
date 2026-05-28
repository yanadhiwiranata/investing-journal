# Workflow Checklist for Ticker Analysis

**Use this checklist before analyzing any ticker.** Reference the appropriate files below to ensure nothing is missed.

## Pre-Analysis (Required Every Time)

- [ ] **Step 0: Data Freshness** (See AGENT.md Step 0)
  - [ ] Verify current date (check system date)
  - [ ] Verify current stock price from latest available sources (SEC filings, IR pages, financial data)
  - [ ] Confirm latest reported quarter from SEC/IR
  - [ ] Search for latest analyst ratings (TipRanks, 5 most recent)
  - [ ] Verify current commodity prices if applicable
  - [ ] Check for post-earnings events (guidance changes, M&A, etc.)
  - [ ] Check for macro shifts relevant to thesis

- [ ] **Step 1: Identify Company & Sector** (See CLAUDE.md "Working with Tickers")
  - [ ] Find company file: `03_sectors/[sector]/companies/TICKER-*.md`
  - [ ] Check watchlist row: `04_portfolio/watchlist/watchlist.csv`
  - [ ] Identify primary exchange and reference price

- [ ] **Step 2: Read Sector Template & Checklist** (See AGENT.md Step 1.5 — MANDATORY)
  - [ ] Identify the sector (gold/silver miners, oil/energy, technology, automotive, solar)
  - [ ] Read `05_templates/[SECTOR]_company_template.md` to understand required sections
  - [ ] Read `05_templates/[SECTOR]_research_checklist.md` to understand key metrics
  - [ ] Read `05_templates/earnings_comparison_template.md` for earnings structure
  - [ ] **Critical:** This defines what a COMPLETE analysis looks like for this sector

## Analysis & Output (Follow Template Structure)

- [ ] **Step 3: Gather Earnings Data** (See AGENT.md Steps 3–4)
  - [ ] Identify latest 3 quarters (current + prior + YoY)
  - [ ] Verify earnings dates and report status
  - [ ] Note if next earnings is already scheduled or upcoming

- [ ] **Step 4: Create/Update Earnings Comparison** (See AGENT.md Step 5)
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

- [ ] **Step 5: Create/Update Company File** (See AGENT.md Step 7)
  - [ ] Use sector template: See list below
  - [ ] File location: `03_sectors/[sector]/companies/TICKER-company-name.md`
  - [ ] **REQUIRED SECTIONS:**
    - **Snapshot** (with current price, current_price_timestamp, and PRICE TARGETS):
      - [ ] Current price + timestamp
      - [ ] **Bear case target** + short description + upside/downside % (e.g., "$45 (-20%): demand collapse, production miss")
      - [ ] **Base case target** + short description + upside/downside % (e.g., "$56 (+5%): in-line execution, commodity stable")
      - [ ] **Bull case target** + short description + upside/downside % (e.g., "$72 (+35%): margin expansion, production beat")
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

- [ ] **Step 6: Update Watchlist** (See AGENT.md Step 8)
  - [ ] File: `04_portfolio/watchlist/watchlist.csv`
  - [ ] Update: `current_price`, `current_price_timestamp`, `reference_price`, `last_analyzed_at`, `status`, `thesis_summary`, `next_review_scheduled_at`
  - [ ] Thesis summary should reflect **trend direction** (momentum, guidance, catalyst timing)

- [ ] **Step 7: Set Next Review Date** (See AGENT.md Step 9)
  - [ ] Default: 5–10 business days before next earnings
  - [ ] Advance if: major catalyst, commodity shock, geopolitical event, guidance change

## Sector Templates to Use

| Sector | Template File | Checklist |
|--------|---------------|-----------|
| **Gold Miners** | `gold_miners_company_template.md` | `gold_miners_research_checklist.md` |
| **Silver Miners** | `silver_miners_company_template.md` | `silver_miners_research_checklist.md` |
| **Oil & Energy** | `oil_energy_upstream_ep_company_template.md` | `oil_energy_upstream_ep_research_checklist.md` |
| **Technology / AI Compute** | `ai_compute_infrastructure_company_template.md` | `ai_compute_infrastructure_research_checklist.md` |
| **Power Infrastructure** | `power_cooling_infrastructure_company_template.md` | `power_cooling_infrastructure_research_checklist.md` |
| **Power Generation / Nuclear** | `power_generation_nuclear_company_template.md` | `power_generation_nuclear_research_checklist.md` |
| **Automotive** | `automotive_company_template.md` | `automotive_research_checklist.md` |
| **Solar Energy** | `solar_energy_company_template.md` | `solar_energy_research_checklist.md` |

## Generic Template (Fallback)

If no sector template exists: `company_analysis_template.md`

## Key Rules

1. **MANDATORY SEQUENCE:** Read WORKFLOW-CHECKLIST → AGENT.md → IDENTIFY SECTOR → READ SECTOR TEMPLATE/CHECKLIST → ANALYZE
   - Skipping the sector template reading (Step 2) results in incomplete analysis
   - The template defines what sections and metrics are required for that sector
2. **Always read AGENT.md first** for ticker-analysis requests (see CLAUDE.md Quick Start)
3. **Read the sector template AND checklist BEFORE analyzing** (Step 2)
   - Gold miners: `gold_miners_company_template.md` + `gold_miners_research_checklist.md`
   - Silver miners: `silver_miners_company_template.md` + `silver_miners_research_checklist.md`
   - Oil/energy: `oil_energy_upstream_ep_company_template.md` + checklist
   - Technology/AI compute: `ai_compute_infrastructure_company_template.md` + checklist
   - Power infrastructure: `power_cooling_infrastructure_company_template.md` + checklist
   - Power generation/nuclear: `power_generation_nuclear_company_template.md` + checklist
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
3. ✅ Use template: `gold_miners_company_template.md`
4. ✅ Create/update: `03_sectors/gold_miners/earnings/CDE/2026-Q1-comparison.md`
   - Include current analyst consensus in structured table
5. ✅ Update: `03_sectors/gold_miners/companies/cde-coeur-mining.md`
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

## Additional Command: Update Macro

**Out of scope:** Do NOT open the watchlist CSV, do NOT look up individual stock prices or ticker data. Only macro indicators and sector-level verdicts.

Use this when the user says `update macro`.

**Hourly Update Protocol (Active Monitoring Days):**
- [ ] **Step 0a - Get real system time (MANDATORY FIRST):** Run `date` in terminal before naming any file
  - Records exact Jakarta HHMM for filename and `**Last Updated:**` header
  - Example: `date` → `Thu May 28 17:36:00 WIB 2026` → use `1736` in filename, `17:36` in header
  - **Never guess or invent the time** — a wrong timestamp breaks the audit trail
- [ ] **Step 0 - Data Freshness Verification:** Refresh all market prices with live sources (do NOT use cached prices)
  - [ ] DXY with exact timestamp
  - [ ] Gold spot price with timestamp
  - [ ] Silver spot price with timestamp
  - [ ] U.S. 2Y and 10Y yields (intraday levels, not just prior close)
  - [ ] WTI and Brent crude with timestamps
  - [ ] Check for any released macro data (NFP — Non-Farm Payrolls; CPI — Consumer Price Index; PPI — Producer Price Index; PCE — Personal Consumption Expenditures, Fed's preferred inflation gauge; jobless claims; etc.)
  - [ ] Check for breaking geopolitical news
- [ ] **Create new hourly file** with timestamp-based naming: `YYYY-MM-DD-HHMM-macro-*.md`
  - Examples: `2026-05-15-0800-macro-top-down-review.md`, `2026-05-15-0900-macro-update.md`
- [ ] **Include Data Freshness Table** showing:
  - Current Level | 24H Change | Intraday Change (Δ) | Status
- [ ] **Include Update Log table** at bottom showing:
  - Time (Jakarta) | Update Summary | Key Changes
- [ ] **Assess macro regime** (stagflation, risk-on, risk-off, etc.)
- [ ] **Identify asset implications:**
  - Is DXY strengthening or weakening?
  - Are yields rising or falling?
  - Is gold under pressure or supported?
  - Is oil holding geopolitical premium?
- [ ] **Next catalyst:** When is the next major data release or event?
- [ ] **Keep ALL hourly files** — do not delete or overwrite previous hours

**Triggers for Mid-Session Update:**
- DXY breaks ±2% intraday
- Gold/silver breaks ±2% intraday
- Yields move ±20 bps intraday
- Oil breaks key levels ($100, $105, etc.)
- Geopolitical news breaks (Iran, Strait of Hormuz, sanctions)
- Fed speaker releases comments or surprise data lands
- Market volatility spikes (VIX > 25)

## Additional Command: Update Today

Use this when the user says `update today` or asks for the full daily maintenance pass.

**⚠️ CRITICAL: Use the SAME hourly timestamp for ALL THREE steps so prices stay synchronized.**

**REQUIRED ORDER & SPECIFICATIONS:**

1. [ ] **Run `update macro` FIRST** (See "Additional Command: Update Macro" above for full protocol)
   - [ ] **Timestamp:** Determine current time in Jakarta (e.g., 08:00)
   - [ ] **Macro filename:** `YYYY-MM-DD-HHMM-macro-today-update.md`
   - [ ] **Example:** `2026-05-15-0800-macro-today-update.md`
   - [ ] **Include:** Data freshness table + Update log + Macro regime assessment
   - [ ] **Do NOT overwrite** previous hourly macro files if running multiple times
   - [ ] **Record timestamp:** Note this HHMM for next two steps

2. [ ] **Run `update watchlist` SECOND** (with MATCHING timestamp from step 1)
   - [ ] Refresh current prices for all active watchlist rows
   - [ ] **Update `current_price_timestamp`:** Use MATCHING time from macro (e.g., `2026-05-15 08:00:00`)
   - [ ] Update `current_price`, `reference_price`, `last_analyzed_at` fields
   - [ ] Compare refreshed prices against `target_buy_zone`
   - [ ] Use macro read to assess thesis staleness

3. [ ] **Run `update porto` THIRD** (with MATCHING timestamp from step 1)
   - [ ] Refresh current prices for all holdings
   - [ ] **Update `current_price_timestamp`:** Use MATCHING time from macro (e.g., `2026-05-15 08:00:00`)
   - [ ] Recalculate `unrealized_pnl` and `unrealized_pnl_pct` based on refreshed prices
   - [ ] Update open orders and completed transactions

4. [ ] **Final Summary** combining all three (use matching timestamp):
   - Macro verdict + next catalyst
   - Watchlist names actionable now vs. needing re-analysis
   - Portfolio P/L changes from price refresh
   - **All data timestamped:** `2026-05-15 08:00:00` (example)

**Timestamp Synchronization Checklist:**
- [ ] Macro file: `2026-05-15-0800-macro-*.md`
- [ ] Watchlist `current_price_timestamp`: `2026-05-15 08:00:00`
- [ ] Portfolio `current_price_timestamp`: `2026-05-15 08:00:00`
- [ ] All three use SAME HHMM (08:00 in this example)
- [ ] Run `update porto` third
- [ ] Keep the sequence intact so macro context informs the watchlist and portfolio updates
- [ ] Return one concise end-of-run summary covering macro changes, watchlist actions, and portfolio record changes

## Additional Command: Update Porto

Use this when the user says `update porto` or asks to update portfolio records.

- [ ] Update `04_portfolio/holdings/holdings_snapshot.csv` for live positions only
  - [ ] **Do NOT include tickers with 0 shares** — holdings should only list active positions
  - [ ] When a position closes to 0 shares, **remove the row entirely** from the CSV
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

## Timestamp Format Rules

**CSV Files (watchlist, holdings, transactions, etc.):**
- Use format: `YYYY-MM-DD HH:MM:SS` (e.g., `2026-05-15 08:00:00`)
- Store timestamps in **Jakarta local time**
- Do **NOT** append timezone label or "Jakarta time" inside the CSV
- Include hour, minute, and second for full precision

**Markdown Files (analysis notes, macro notes, etc.):**
- File names may include timestamps: `YYYY-MM-DD-HHMM-macro-top-down-review.md`
- Headers may include "Jakarta time" for clarity: `**Last Updated:** 2026-05-15 08:00 Jakarta time`
- This helps readers understand timezone context without cluttering the CSV data

## ⚠️ CRITICAL REMINDERS

- **READ THE SECTOR TEMPLATE FIRST (Step 2).** This is not optional. The template defines the required structure and metrics for that sector.
- **If you skip the template reading, your analysis will be incomplete.** You may miss required sections (e.g., Operating Quality, Guidance, Analyst Consensus).
- **The template is the specification.** If the template requires 10 sections and the company file has 8, your update is incomplete.

---

**Always complete this checklist before claiming analysis is done.**
