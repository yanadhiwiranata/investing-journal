# Investing Journal

This project is built to simplify my current investing workflow and make it easier to see whether my fundamental checking process is improving every day.

Instead of keeping research scattered across notes and ad hoc files, this repository is meant to give me a repeatable structure for:

- capturing research inputs quickly
- reviewing company fundamentals in a consistent way
- comparing results across quarters
- tracking whether my analysis is getting clearer, faster, and more complete over time

## Core Goal

The main goal is not just to store notes. The goal is to build a daily working system for fundamental analysis.

That means this repo should help me:

- reduce friction in my current workflow
- standardize how I review companies
- avoid missing important checks
- measure improvement in the quality of my fundamental review process

## How This Repo Helps

- `00_inbox/` is the landing area for raw inputs before they are organized.
- `01_strategy/` stores investing principles, rules, and process improvements.
- `02_market/` keeps market-level and macro notes that affect decision-making.
- `03_sectors/` is where sector research and company-specific work will live.
- `04_portfolio/` is for holdings, watchlists, and portfolio decision tracking.
- `05_templates/` contains the repeatable templates and checklists for company research.
- `06_reviews/` is for reviewing what worked, what was missed, and what to improve.
- `07_archive/` keeps older material that is no longer active but still worth saving.
- `AGENT.md` defines the default AI workflow for analyzing a company and updating research consistently.
- `CLAUDE.md` provides the same repo context for Claude-based workflows.

Together, these files act as the operating system for my daily fundamental work.

## Prompt Reminder

When I want the agent to follow the full process without skipping steps, use this exact prompt:

`full CDE`

Meaning:

`Analyze CDE using the correct templates and complete the full checklist and workflow without skipping any steps.`

Another useful maintenance command:

`update watchlist`

Meaning:

`Refresh current prices in watchlist.csv, update current_price and current_price_timestamp with full Jakarta-time precision, compare them with target buy zones, and tell me which names are close enough to entry or need re-analysis first.`

Another useful market workflow:

`update macro`

Meaning:

`Run an upgraded macro refresh: review geopolitics, DXY, U.S. bond yields, commodity trends, save a macro note, and add hour-level monitoring notes when volatility is high or a key release is close.`

Another useful portfolio workflow:

`update porto`

Meaning:

`Update current holdings, open orders, and completed trades across the portfolio CSV files.`

Another useful daily workflow:

`update today`

Meaning:

`Run the daily sequence in order: update macro first, then update watchlist, then update porto.`

Sector rotation entry checks:

`entry check gold`

Meaning:

`Verify whether gold miner Layer 1 entry is viable today by checking real yields, DXY, gold price, and analyst consensus. Recommend entry price and size if gates pass, or explain blockers.`

`entry check silver`

Meaning:

`Verify whether silver miner Layer 1 entry is viable today by checking ISM, copper, industrial demand, risk sentiment, and analyst consensus. Silver has more complex gates than gold.`

`entry check both`

Meaning:

`Run full macro checklist for both gold and silver miners. Recommend which to enter first, or skip both if conditions unfavorable. Compare current state vs. entry thresholds.`

Full command documentation:

- `COMMANDS.md`

## Working Principle

Everyday use should make the process:

- simpler than my current manual workflow
- more consistent from one company review to the next
- easier to compare across time
- easier to improve through repetition

## Current Focus

Right now, the most important part of this repository is the template system in `05_templates/`.

Those templates are here to help turn fundamental analysis into a clearer routine instead of a loose note-taking habit.

## Current Folder Structure

### `00_inbox/`

Goal: capture rough ideas, links, notes, screenshots, and unfinished research before sorting them into the proper place.

### `01_strategy/`

Goal: define the investing framework, rules, checklist standards, and process improvements that guide daily work.

Key files:
- `position_management_framework.md` — Core discipline for avoiding FOMO and price chasing
- `gold_miners_sector_rotation_rules.md` — Macro-driven entries (real yields primary), 100-share Layer 1
- `silver_miners_sector_rotation_rules.md` — Demand-driven entries (ISM primary), 50–75-share Layer 1
- `ENTRY_DECISION_CHECKLIST.md` — Pre-entry verification for both gold and silver (required before Layer 1)
- `gold_vs_silver_miners_quick_comparison.md` — Decision tree for choosing which sector to enter when

### `02_market/`

Goal: track broader market context, macro themes, rates, commodities, and other top-down signals that affect fundamental analysis.

### `03_sectors/`

Goal: organize research by sector so company analysis, earnings notes, and sector comparisons stay grouped and easier to review.

### `04_portfolio/`

Goal: monitor holdings, watchlists, position ideas, and decision history in one place.

### `05_templates/`

Goal: provide reusable templates and research checklists so every company can be reviewed with a more consistent process.

### `06_reviews/`

Goal: review past work regularly, identify mistakes or missed signals, and measure whether the fundamental checking process is improving.

### `07_archive/`

Goal: keep older notes, inactive ideas, or completed materials without cluttering the active workflow.
