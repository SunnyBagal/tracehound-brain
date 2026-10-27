# seed-01a: stop report (Step 2 first-PR gate)

Stopped at the first-PR gate: run green, no review posted, so the model header could not be confirmed. Ruling since received: continue with a 3-PR gate.

## Step 0: all pass
- Pre-registration commit a4a6c83 in history; key.json and SCORING.md sha256 match the published hashes.
- Scorer offline checks: 13/13 pass. Scorer makes no model calls.
- gh: SunnyBagal, ADMIN on both mirrors; NEBIUS_API_KEY secret present on both.
- Both mirrors: default branch main, no TraceHound workflow, 0 PRs; seed branches 15/15 each (none missing, none extra).

## Step 1
- Workflow committed on main: linea-seeded 3012a54, recall-seeded 296f8f3 (identical to spec). Actions already enabled; no change.

## Step 2
- Disclosed script copy runs/open-prs-seed01a.sh, tracehound-seeds dbd3bdd: (a) no CodeRabbit trigger, (b) 90 s gap, (c) cd to repo root (needed because the copy lives in runs/). Order unchanged.
- PRs opened: 1 of 30. https://github.com/SunnyBagal/recall-seeded/pull/1
- Run 40556700220: green. https://github.com/SunnyBagal/recall-seeded/actions/runs/40556700220
- Log: model nvidia/Nemotron-3.5-Super-VL, dry-run false, reviewer pool-verify-v1; reviewer step 13.5 s; "reviewer returned no findings; nothing posted."
- Reviews / inline / conversation comments on the PR: 0 / 0 / 0. No other review bots appeared.
- PR diff: 2 files, +4 / -3 (non-zero, so the zero-chunk case does not apply).
- Nebius spend: not shown in the run log.
