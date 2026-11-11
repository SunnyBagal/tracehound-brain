# Ledger vs the Nebius console: billing lag and the off-ledger gap

Source and authority: [docs/budget.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/budget.md) (2026-11-09). Every number below is from that file; the arithmetic is its own. Spend history: [costs](costs.md).

## What happened (2026-11-08 → 09)
1. **8 Nov:** the owner read **$23.39** in the console, and `budget.md` split it into job lines.
2. **9 Nov:** the console showed **$19.31**, where the ledger predicted about $22.90. That looked like about $3.59 spent off-ledger overnight.
3. **The response:** the old API key was revoked, a leak audit ran, and **all paid work was frozen**.
4. **What the leak audit found:**
   - codex found no CI run that could reach the model in the 48 h before;
   - grok found no committed key in 53 public repos.

## The finding: billing lag, not a leak (likely, not proven)
- **074's charge:** job 074 ran on 8 Nov from 08:43:04Z to 09:39:08Z and cost **$3.36463** by the ledger.
- **The reading's timing:** the $23.39 reading came about one minute after 074 finished, so it did not yet include 074.
- **Corrected arithmetic:** 23.39 − 3.36463 (074) − 0.49090 (075) = **$19.53** expected, against **$19.31** real.
- **What's left:** $0.22 unexplained, about 6% of the two jobs. That is within a price-estimate error, but not proven.
- **New rule (budget.md rule 5):** compare ledger and console **at least 2 h after a paid job ends**, and record the reading's UTC time with the balance.

## The older gap that remains
The local ledger (`.tracehound/spend.jsonl`) does not see every charge.
- **The gap:** from the $43.52 anchor (`docs/bench/reviewbench.md`) to $23.39, the real balance fell $20.13. Corrected for the lag, $19.40 of that is not in the local ledger.
- **CI explains part of it.** The GitHub Action keeps its ledger in the runner's temp dir, which is discarded. Since 27 Oct, 89 workflow runs reached the model, none after 1 Nov. At $0.075 per PR that is an estimated **$5.18–6.68**.
- **Other ledgers explain none of it.** codex audited 72 `spend.jsonl` files under `~/Projects`, and they demonstrate no extra non-test spend.
- **Unexplained:** **$12.7–14.2** after CI. Only the console's usage by day and model can settle it.

## Rules that came out of it (budget.md "Rules")
- No experiment without a line in `budget.md`.
- A global hard stop sits $1 below the real balance.
- Calls from CI or other machines need their own line and a console check.
- CI reviews are manual since 2026-11-09 (dispatch or the `tracehound-review` label), and each run counts as an experiment.

**Status (2026-11-11):** the spend freeze holds until Sunny confirms the hourly console usage. Money left by budget.md is $19.31.

Lesson: [lessons](../lessons.md) (2026-11-09, billing lag).
