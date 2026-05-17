# Sector Rotation Rules & Entry Checklists

This folder contains disciplined, macro-driven entry and exit frameworks for tracking sector rotation across five main investment themes.

## Structure

```
sector_rotation/
├── ENTRY_DECISION_CHECKLIST.md       ← Shared checklist for all sectors
├── gold_miners/
│   ├── rotation_rules.md             ← Gold-specific macro rules, layer entry, exits
│   └── vs_silver_comparison.md       ← Decision tree: when to enter gold vs silver
├── silver_miners/
│   └── rotation_rules.md             ← Silver-specific demand/ISM rules, layer entry, exits
├── oil_energy/                       ← (in development)
├── automotive/                       ← (in development)
├── technology/                       ← (in development)
└── solar_energy/                     ← (in development)
```

## Workflow

1. **Check `ENTRY_DECISION_CHECKLIST.md`** first for the macro gate framework (real yields, DXY, ISM, copper, analyst consensus)
2. **Review sector-specific `rotation_rules.md`** for:
   - Layer 1/2/3 entry calculations
   - Position sizing and standing sell orders
   - Daily alarm triggers (macro circuit breakers)
   - Exit rules (profit-taking, invalidation, sector rotation)
3. **For gold vs silver decisions**, use `gold_miners/vs_silver_comparison.md` to decide which to enter first

## Key Principles

- **Macro is primary**: Real yields, DXY, and commodity prices drive entries more than company metrics
- **Layered accumulation**: 3-tier entry system reduces emotionality and improves fill discipline
- **Standing sells**: Automatic profit-taking at +14–18% locks gains and removes FOMO
- **Stop losses**: Hard stops below Layer 1 entry prevent thesis deterioration
- **No price chasing**: Orders resting at discounts to analyst consensus; never buy strength

## Commands Using These Files

- `entry check gold` — Verify gold miner Layer 1 viability
- `entry check silver` — Verify silver miner Layer 1 viability
- `entry check both` — Compare both sectors and recommend entry order

---

**Last Updated:** 2026-05-17  
**Status:** Gold & silver frameworks active; other sectors in development
