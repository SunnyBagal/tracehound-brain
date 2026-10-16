# Costs and caps

All Nebius spend is hackathon credit, recorded in the ledger (`.tracehound/spend.jsonl`, `pnpm spend`). **Prices in `config/prices.json` are third-party estimates, not confirmed in the Nebius console**, so every figure here is an estimate.

## Caps

| Cap | Value | Where |
|---|---|---|
| Per-run (code default) | $1 | `TRACEHOUND_BUDGET_RUN_USD` |
| Total (code) | $45 | `TRACEHOUND_BUDGET_TOTAL_USD` |
| Per relay run | $3 (set 14 Oct) | `~/Projects/tracehound-ops/ops/lanes.json` line 11, `nebiusCapUsd` |
| Relay's Claude spend per run | $30 | relay config (reported in relay output) |

## Ledger over time (cumulative)

| When | Ledger | What moved it |
|---|---|---|
| 2026-10-04 | $3.024 | naming, repair agent, reproduce stage |
| 2026-10-05/06 | +$6.27 | held-out evaluation, 160 counted runs ($5.72) plus void runs |
| after 056 (15 Oct) | $10.41140 | 054 judge validation, 055 baseline, 056 verifier |
| after 057 | $11.26866 | +$0.85726 (reviewer $0.83130, judge $0.02596) |
| after 058 | $11.60211 | +$0.33345 (verifier $0.32254, judge $0.01091) |
| after 059 | — | expected under $1 |

Headroom: about $33 of the $45 total cap.

## Unit costs (from results docs)

- review-diff-v1 Super: about $0.012 per PR. review-ctx-v1: about $0.021 per PR.
- verify-ctx-v1: about $0.002 per finding, about $0.008 per PR on dev.
- Judge: about $0.026 per 40-PR scoring.
- Rough full pool pipeline (diff + ctx + verifier): 3–5 cents per PR. 059 reports the measured number.

Open: an own-money cap for non-Nebius comparison arms, if any are ever run (none planned). Nemotron Ultra only if its Nebius price is verified.
