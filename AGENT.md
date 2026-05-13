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
4. Verify current price and record the exact `price_as_of` date.
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

Secondary summaries can help with context, but not as the final authority when a primary source exists.

## Default Command Behavior

When the user provides only a ticker and a verb such as `analyze`, `review`, or `update`, interpret it as a full research workflow unless the user says otherwise.

Example:

- `analyze EXK`
- `update MTDR`
- `review HL`

For these requests, assume the user wants the most recent financially relevant information, not just an update to old notes. Refresh the facts first.

When the user pastes a macro or calendar screenshot, interpret it as a macro-impact workflow unless the user says otherwise.

Examples:

- pasted TradingView U.S. economic calendar screenshot
- pasted screenshot of yields, DXY, gold, and silver
- `analyze this calendar`

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

- `03_sectors/gold_silver_miners/`
- `03_sectors/oil_energy/`
- any watchlist names most exposed to the move

## Macro Note Output Standard

When working from a pasted macro screenshot, the markdown note should include these sections:

1. Screenshot Summary
2. Event Table
3. What Beat / Missed vs Forecast
4. Market Impact Logic
5. Gold Impact
6. Silver Impact
7. DXY and Yield Impact
8. Cross-Asset Trend Read
9. What To Watch Next
10. Bottom Line

## Primary Workflow: Analyze a Ticker

### ⚠️ STEP 0: DATA FRESHNESS VERIFICATION (MANDATORY — DO THIS FIRST)

**This step is non-negotiable. Do not skip or defer.** Financial analysis with stale data produces incorrect conclusions and poor recommendations.

**Current date context:** Today is 2026-05-14. All analysis must reference this date explicitly.

**Freshness checklist — VERIFY ALL BEFORE PROCEEDING:**

1. **Current Stock Price**
   - Search for ticker's current price with exact date and time
   - Record `price_as_of: YYYY-MM-DD HH:MM` (not just date)
   - Example: "MTDR stock price $57.31 as of May 13, 2026 close" → `price_as_of: 2026-05-13`

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
- **Stock price:** WebSearch for "TICKER stock price May 2026" + check Yahoo Finance or TradingView
- **Earnings date:** Search for "TICKER Q1 2026 earnings date" or visit company IR site
- **Analyst ratings:** WebSearch "TICKER analyst ratings May 2026" + check TipRanks
- **Commodities:** WebSearch for "WTI crude oil price May 14 2026" or visit oilprice.com / NYMEX / spot gold sites
- **Macro:** WebSearch for "US 10Y yield May 2026" + "US PMI May 2026"
- **News:** Check company press releases, 8-K filings, and financial news for last 30 days

**Example of what freshness failure looks like:**
❌ WRONG: "Oil is around $73/Bbl (from Q1 2026 analysis)" → Later discover WTI at $102 (40% difference!)
❌ WRONG: "Latest quarter Q1 2026" → Don't verify if Q2 was already reported
❌ WRONG: "Analyst consensus $61" → Miss that Truist just upgraded to $67 yesterday
✅ RIGHT: "Current price $57.31 (May 13, 2026 close), WTI $102.21 (May 13), analyst consensus ~$63-67 post-Truist upgrade (May 12), latest quarter Q1 2026 (reported May 6)"

**If you skip this step:** You will present outdated financial conclusions, miss material catalysts, and the analysis will be stale within days. The MTDR case study showed oil at $73 was 40% wrong vs. current $102.

---

### 1. Identify the company and sector

Search for an existing company note:

- `03_sectors/[sector]/companies/TICKER-*.md`
- `04_portfolio/watchlist/watchlist.csv`

Current sector folders in use:

- `03_sectors/gold_silver_miners/`
- `03_sectors/oil_energy/`
- `03_sectors/technology/`
- `03_sectors/automotive/`
- `03_sectors/solar_energy/`

If the company does not exist yet:

- infer the most likely sector
- create the company note in the matching `companies/` folder
- use the correct sector template from `05_templates/`

Before starting the analysis, check whether the ticker already exists in the watchlist.

If it exists, use these watchlist fields as the default market reference:

- `exchange`
- `reference_price`
- `price_as_of`

Use them to:

- identify the primary listed exchange to analyze
- anchor the starting price context in the company note and earnings comparison
- compare current research conclusions against the last tracked watchlist price

Use the primary listed exchange to keep currency formatting consistent:

- `NYSE` / `NASDAQ` names: use `USD ($)` for current price, entry zones, price targets, and short-term price scenarios
- If the company reports earnings in another currency, keep reported financials and guidance in that local currency, but add `USD` alongside when useful for readability and consistency

If the watchlist has no entry yet:

- create one by the end of the analysis
- populate `exchange`, `reference_price`, and `price_as_of`

### 2. Identify the company and sector (if not already done in Step 1)

This is now Step 2 (was Step 1 before freshness integration).

Search for an existing company note:

- `03_sectors/[sector]/companies/TICKER-*.md`
- `04_portfolio/watchlist/watchlist.csv`

Current sector folders in use:

- `03_sectors/gold_silver_miners/`
- `03_sectors/oil_energy/`
- `03_sectors/technology/`
- `03_sectors/automotive/`
- `03_sectors/solar_energy/`

If the company does not exist yet:

- infer the most likely sector
- create the company note in the matching `companies/` folder
- use the correct sector template from `05_templates/`

Before starting the analysis, check whether the ticker already exists in the watchlist.

If it exists, use these watchlist fields as the default market reference (from Step 0 freshness work):

- `exchange`
- `reference_price` (from Step 0 — this should now be refreshed)
- `price_as_of` (from Step 0 — update to today's date)

---

### 3. Use Step 0 freshness data to determine the reporting quarter set

**You have already verified the latest quarter in Step 0.** Now use that information to build the quarter set.

For earnings-driven analysis, always gather these three periods:

- Latest reported quarter (verified in Step 0)
- Immediately previous quarter
- Same quarter one year earlier

Examples:

- If latest is `Q1 2026` (verified in Step 0), gather `Q1 2026`, `Q4 2025`, and `Q1 2025`
- If latest is `Q3 2026`, gather `Q3 2026`, `Q2 2026`, and `Q3 2025`

For earnings-driven analysis, always gather these three periods:

- Latest reported quarter
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
  - `price_as_of`
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

- `05_templates/gold_silver_miners_company_template.md`
- `05_templates/oil_energy_upstream_ep_company_template.md`
- `05_templates/technology_company_template.md`
- `05_templates/automotive_company_template.md`
- `05_templates/solar_energy_company_template.md`

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

- latest verified price and `price_as_of`
- latest reported quarter and report date
- next earnings date if known
- key post-earnings development since the latest quarter
- whether the thesis is improving, stable, or deteriorating right now

### 8. Update watchlist if appropriate (was Step 8)

If the company is actively being monitored or the user asked for a fresh analysis, update:

- `04_portfolio/watchlist/watchlist.csv`

Focus on:

- `exchange`
- `reference_price`
- `price_as_of`
- `status`
- `thesis_summary`
- `next_review`

Watchlist schema:

- `date_added`
- `ticker`
- `company`
- `exchange`
- `reference_price`
- `price_as_of`
- `sector`
- `theme`
- `status`
- `entry_idea`
- `target_buy_zone`
- `thesis_summary`
- `next_review`

### 9. Set next review timing (was Step 9)

Default next review date:

- 5 to 10 business days before the next expected earnings release

Bring the review date forward if there is an important catalyst before earnings, such as:

- guidance update
- commodity move
- product launch
- mine ramp milestone
- regulatory event

## Sector Routing Rules

Use the company’s operating model to choose the template.

### Gold and Silver Miners

Use:

- `05_templates/gold_silver_miners_company_template.md`
- `05_templates/gold_silver_miners_research_checklist.md`

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

1. Snapshot
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

## If the user says only "analyze TICKER"

Do all of the following by default:

**0. STEP 0: DATA FRESHNESS VERIFICATION (MANDATORY FIRST)**
   - Verify current date, latest reported quarter, current stock price, commodity prices, analyst ratings, post-earnings events, macro indicators
   - Search for fresh data; do NOT rely on cached model knowledge
   - Record all prices/dates with exact timestamps

1. Find the sector and existing company file
2. Identify the company and sector (pull watchlist data)
3. Determine the reporting quarter set using freshness data from Step 0
4. Download the latest quarter, previous quarter, and same quarter last year materials
5. Save files under the correct earnings folder
6. Create or update the multi-quarter comparison note
7. Create or update the company analysis note using the sector-specific template
8. Update the watchlist entry if appropriate (use Step 0 current price and dates)
9. Set the next review date

## Templates to Use

- `05_templates/company_analysis_template.md`
- `05_templates/earnings_comparison_template.md`
- `05_templates/gold_silver_miners_company_template.md`
- `05_templates/gold_silver_miners_research_checklist.md`
- `05_templates/oil_energy_upstream_ep_company_template.md`
- `05_templates/oil_energy_upstream_ep_research_checklist.md`
- `05_templates/technology_company_template.md`
- `05_templates/technology_research_checklist.md`
- `05_templates/automotive_company_template.md`
- `05_templates/automotive_research_checklist.md`
- `05_templates/solar_energy_company_template.md`
- `05_templates/solar_energy_research_checklist.md`

## Practical Example

If the user says `analyze EXK`, the agent should:

1. Locate `03_sectors/gold_silver_miners/companies/EXK-endeavour-silver.md`
2. Confirm the latest reported quarter
3. Save the latest report, prior quarter report, and same quarter prior year report under `03_sectors/gold_silver_miners/earnings/EXK/[quarter]/`
4. Create or refresh `03_sectors/gold_silver_miners/earnings/EXK/[YYYY-QN]-comparison.md`
5. Extract the latest guidance and compare it with prior commentary
6. Update the EXK company memo using the gold and silver miners template
7. Update watchlist status and next review date if needed
