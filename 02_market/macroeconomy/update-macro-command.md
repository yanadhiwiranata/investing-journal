# Update Macro Command

Use this file as the dedicated workflow reference for the `update macro` command.

## Purpose

Run `update macro` when you want a structured top-down market review focused on:

- geopolitics
- DXY
- U.S. bond yields
- real yields when relevant
- commodity trends
- the next important U.S. economic releases

This command is meant to answer:

- What is the current macro regime?
- Is the setup supportive or hostile for the portfolio?
- Which upcoming release or event matters most next?
- When should the next macro review happen?

## Output Location

Save the review under:

- `02_market/macroeconomy/`

Preferred filename:

- `YYYY-MM-DD-macro-top-down-review.md`

Use this template:

- `05_templates/macro_top_down_review_template.md`

## Required Review Areas

### 1. Geopolitical Update

Check for developments that can materially affect:

- inflation expectations
- energy supply
- shipping routes
- sanctions or export controls
- industrial supply chains
- global risk appetite

Do not just list headlines. Classify the development as:

- short-lived noise
- medium-term driver
- structural regime change

### 2. Dollar and Rates

Refresh and interpret:

- DXY
- U.S. 2Y Treasury yield
- U.S. 10Y Treasury yield
- real yields if relevant
- curve behavior if relevant

For each, state:

- current level
- short-term trend
- main driver
- what would confirm the move
- what would invalidate the move

### 3. Commodity Trend Review

Always check:

- gold
- silver
- WTI
- Brent

Check other commodities when relevant to active positions or watchlist names:

- natural gas
- copper
- uranium
- other portfolio-relevant commodities

For each, state:

- current level
- current trend
- main driver
- DXY / yield sensitivity if relevant

### 4. U.S. Economic Calendar

Review only the next releases that could realistically change the macro view.

Prioritize:

- CPI
- PPI
- NFP / unemployment
- retail sales
- ISM Manufacturing / Services PMI
- FOMC decisions
- important Fed speeches
- Treasury auctions when yields are the main driver

For each event, record:

- exact date
- exact time
- why it matters
- which assets are most exposed

When this workflow leads to a watchlist refresh, use `current_price_timestamp` with full time precision:

- `YYYY-MM-DD HH:MM:SS`
- use Jakarta local time in the watchlist
- do not write the timezone suffix inside the CSV

## Required Final Verdict

End the review with a practical market read:

- bullish / neutral / bearish for DXY
- bullish / neutral / bearish for yields
- bullish / neutral / bearish for gold
- bullish / neutral / bearish for silver
- bullish / neutral / bearish for oil
- risk-on / neutral / risk-off

Also state:

- the most important next macro date
- the biggest invalidation trigger
- which sectors or watchlist names are most exposed

## Next Review Schedule

Use this rule set:

- If the next major U.S. release is within 1 to 3 trading days, schedule the next review for the day before or the morning of the release.
- If markets are calm and no major release is close, run a weekly macro review.
- If DXY, yields, gold, silver, or oil move sharply, bring the review forward immediately.
- If geopolitics escalates materially, do not wait for the scheduled review date.

## Suggested Trigger List

Bring the review forward immediately when any of these happen:

- CPI or NFP surprise
- FOMC tone shift
- 10Y yield breakout
- DXY breakout
- gold or silver breakdown / breakout
- WTI or Brent spike or collapse
- war escalation, sanctions shock, or shipping disruption

## Quick Command Definition

`update macro`

Meaning:

`Review geopolitics, DXY, U.S. bond yields, and commodity trends, save a macro note, and set the next review date based on the upcoming U.S. economic calendar.`
