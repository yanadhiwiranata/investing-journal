# Changes Summary: Gold vs Silver Miner Split

**Date:** 2026-05-17  
**Reason:** Gold and silver miners have different macro drivers and behavior patterns. Split into separate, professional frameworks.

---

## New Files Created (4 New + 1 Decision Checklist)

### 1. **Gold Miners Framework** (`gold_miners_sector_rotation_rules.md`)
- 15 sections with gold-specific rules
- Macro driver: Real yields (primary) + DXY (secondary)
- Entry: Layer 1 (100 shares) at analyst consensus –7%
- Position weight cap: <40% of portfolio
- Hold period: 4–8 weeks typical
- Profit-take: +20–25%
- Sample trade updated: CDE scenario showing May 15 real yield shock (SKIP entry)

### 2. **Silver Miners Framework** (`silver_miners_sector_rotation_rules.md`)
- 15 sections with silver-specific rules
- Macro driver: ISM (primary) + Copper (canary) + Industrial demand
- Entry: Layer 1 (50–75 shares) at analyst consensus –9% (deeper discount)
- Position weight cap: <25% of portfolio (lower due to volatility)
- Hold period: 2–4 weeks typical (shorter, more volatile)
- Profit-take: +15–18% (earlier than gold)
- Sample trade updated: AG scenario showing copper weakness signal (SKIP entry)

### 3. **Comparison Card** (`gold_vs_silver_miners_quick_comparison.md`)
- Side-by-side decision table (when to enter which)
- Macro event playbook (CPI, ISM, Fed, copper, etc.)
- Position sizing comparison
- Memory aids (GLD, SIL)
- Current status (May 17): SKIP BOTH (real yield spike + demand softening)

### 4. **Entry Decision Checklist** (`ENTRY_DECISION_CHECKLIST.md`)
- 4-step workflow (5 + 10 + 2 + 3 minutes)
- Mandatory pre-entry verification gates
- Real yields check → ISM check → Copper check → Analyst consensus
- Fillable order template for placing Layer 1
- Post-entry monitoring schedule (gold weekly, silver every 2–3 days)
- Current May 17 verdict: SKIP BOTH (wait for CPI/PMI clarity)

### 5. **Current Macro Conditions** (`macro_may_2026_rebound_headwind.md`)
- Real yields spiked to +0.3% on May 15 (Fed rate-hike probability)
- Expected further weakness May 22+ with CPI data
- Gold: $2,305 (down $75); Silver: $29.10 (down $1.10)
- Copper: $4.52 (down –1.2% from May 15)
- Re-entry triggers documented
- Discipline note: Why silver more vulnerable in rising-rate environment

---

## Files Updated (Links Added)

### 1. **position_management_framework.md**
- **Added:** Links to new gold/silver frameworks at top
- **Links:** `gold_miners_sector_rotation_rules.md`, `silver_miners_sector_rotation_rules.md`, `ENTRY_DECISION_CHECKLIST.md`, `gold_vs_silver_miners_quick_comparison.md`

### 2. **COMMANDS.md**
- **Added:** 3 new entry commands to Quick Command List
  - `entry check gold` — Verify gold miner entry viability
  - `entry check silver` — Verify silver miner entry viability
  - `entry check both` — Run full checklist for both; recommend which to enter first
- **Added:** New "Entry Commands (Sector Rotation)" section with detailed specs for each command

### 3. **README.md**
- **Added:** 3 new command examples in "Prompt Reminder" section
- **Updated:** `01_strategy/` folder description to list key framework files

---

## Files to Archive or Remove (Optional)

The following files are **superseded** by the split frameworks. Consider archiving or deleting:

- `gold_silver_miners_sector_rotation_rules.md` — Original combined framework (replaced by separate gold + silver files)
- `gold_silver_miners_quick_reference.md` — Original combined quick reference (replaced by separate comparison card + checklist)

---

## Memory Index Updated

New entries added to `/Users/hf/.claude/projects/-Users-hf-project-yan-trading/memory/MEMORY.md`:

1. `strategy_gold_miners_rules.md` — Concise gold miner reference
2. `strategy_silver_miners_rules.md` — Concise silver miner reference
3. `macro_may_2026_rebound_headwind.md` — Current headwind + re-entry triggers

---

## Key Behavioral Changes

### Gold Miners
- ✅ Simpler entry gate (just real yields + DXY + analyst)
- ✅ 100-share Layer 1 (stable, visible in order flow)
- ✅ Longer holds (4–8 weeks; macro-driven)
- ✅ Take profits at +20–25% (don't hold for more)

### Silver Miners
- ⚠️ Complex entry gate (ISM + copper + industrial demand all required)
- ✅ Smaller Layer 1 (50–75 shares; leverage is 2.5–3x)
- ✅ Shorter holds (2–4 weeks; high volatility)
- ✅ Take profits earlier (at +15–18%; reversals are fast)
- ⚠️ More circuit breakers (ISM, copper, auto/solar production)

---

## How to Use Tomorrow

1. **Print:** `ENTRY_DECISION_CHECKLIST.md`
2. **Before 9:30 AM:** Run through 4-step checklist (15 minutes)
3. **Decision:** Which sector(s) to enter, or SKIP both
4. **Place order:** Use template section if entering
5. **Monitor:** Gold weekly, silver every 2–3 days

---

## Related Context (Memory)

- [[gold-miners-rotation-rules]] — Gold framework reference
- [[silver-miners-rotation-rules]] — Silver framework reference
- [[gold-vs-silver-comparison-card]] — Decision tree
- [[macro-may-2026-rebound-headwind]] — Current conditions + headwind through CPI May 22

---

**Next Action:** 
- Review checklist before next entry attempt
- Monitor CPI May 22 + PMI China May 23 (inflection points)
- Re-assess entry viability after macro clarity

