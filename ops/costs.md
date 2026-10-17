# Costs and caps

All Nebius spend is hackathon credit, recorded in the ledger (`.tracehound/spend.jsonl`, `pnpm spend`). **Prices in `config/prices.json` are third-party estimates, not confirmed in the Nebius console**, so every figure here is an estimate.

## Real balance

- **2026-10-17: $46.22 left in the Nebius Token Factory console** (Sunny). The local ledger shows $12.45 spent. Its prices are third-party estimates, so the console figure is the truth.
- **GitHub Action runs (dogfood, demo) spend from the same key but are not in the local ledger.** Each run starts with a fresh `.tracehound/`. Check the console after demo days.

## Caps

| Cap | Value | Where |
|---|---|---|
| Per-process (code default) | $1; 062's commands set **$15** in their own environment | `TRACEHOUND_BUDGET_RUN_USD` |
| Total (code) | **$45 default, kept** (17 Oct: ledger $12.45 + $15 for 062 fits; the earlier plan to raise it to $52 was dropped) | `TRACEHOUND_BUDGET_TOTAL_USD`. Leaves about $14 of the real balance for Action runs and price error |
| Per relay run | 3 (14 Oct) → 5 (060) → **15 for 062** | `~/Projects/tracehound-ops/ops/lanes.json` line 11, `nebiusCapUsd` |
| Relay's Claude spend per run | $30 | relay config (reported in relay output) |

## Ledger over time (cumulative)

| When | Ledger | What moved it |
|---|---|---|
| 2026-10-04 | $3.024 | naming, repair agent, reproduce stage |
| 2026-10-05/06 | +$6.27 | held-out evaluation, 160 counted runs ($5.72) plus void runs |
| after 056 (15 Oct) | $10.41140 | 054 judge validation, 055 baseline, 056 verifier |
| after 057 | $11.26866 | +$0.85726 (reviewer $0.83130, judge $0.02596) |
| after 058 | $11.60211 | +$0.33345 (verifier $0.32254, judge $0.01091) |
| after 059 | $11.81728 | +$0.21517 (verifier $0.18675, judge $0.02842) |
| after 060 | $12.45236 | +$0.63508 (Qwen judge $0.0757, Super validity $0.3983, Qwen validity $0.1610) |

Headroom: about $32.5 of the $45 total cap.

## Spending plan (2026-10-17)

| Item | Cap |
|---|---|
| 062: Cal.com run 1 (primary) plus both judges | $6 |
| 062: Cal.com runs 2–3 (spread) | $5 |
| 062: dev runs 2–3 (spread) | $4 |
| GitHub Action: dogfood + 5 demo PRs + judges' window to 15 Dec | about $3 real (not in the ledger) |
| Unallocated buffer | about $25 (no new arms planned; a post-hackathon ReviewBench run is a candidate) |

Product pipeline (pool-verify-v1, 059): median $0.0317, max $0.2308 per PR.

## Unit costs (from results docs)

- review-diff-v1 Super: about $0.012 per PR. review-ctx-v1: about $0.021 per PR.
- verify-ctx-v1: about $0.002 per finding, about $0.008 per PR on dev.
- Judge: about $0.026 per 40-PR scoring.
- Rough full pool pipeline (diff + ctx + verifier): 3–5 cents per PR. 059 reports the measured number.

Open: an own-money cap for non-Nebius comparison arms, if any are ever run (none planned). Nemotron Ultra only if its Nebius price is verified.
