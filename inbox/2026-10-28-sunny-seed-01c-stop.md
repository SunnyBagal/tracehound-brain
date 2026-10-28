# seed-01c: stop report (Linea re-trigger after the 2.1 repin)

Stopped: the linea-seeded#1 re-trigger failed at install again, but it ran the old pin (0aeef06), not 2.1. Ruling since received: open the 14 remaining Linea PRs now; re-trigger #1 once its merge commit rebuilds (or re-set its base once).

## Recall (Action 0aeef06, review-freeze-2): Step 3 done
- PRs: 15 of 15 open, https://github.com/SunnyBagal/recall-seeded/pulls
- Runs: 15 green on first attempt, 0 red, 0 reruns.
- Reviews posted: 8 (5 with 1 inline finding, 3 with 0 findings). Partial reviews: 0.
- Green with no review posted: 7 (#1, #5, #8, #11, #13, #14, #15), reviewer steps 13.5-132.2 s.
- Every posted header names nvidia/Nemotron-3.5-Super-VL.
- Devin: no comments on recall-seeded#3-#15 after access was removed.
- Not scored yet (scoring after all 30).

## Linea
- Workflow repinned to review-freeze-2.1 (86fd5d7), uses: line only: linea-seeded 6d8771a. recall-seeded unchanged at 0aeef06.
- linea-seeded#1 re-triggered (draft, then ready). Run 40589574980: red at pnpm install, but it downloaded TraceHound@0aeef06.
- Cause: the PR's test-merge commit had not been rebuilt (merge_commit_sha affb271, parents 3012a54 + 88fa083; base.sha 3012a54), and a pull_request run takes its workflow from that commit.
- Linea PRs open: 1 of 15.

## Other
- Nebius spend: not shown in the run logs.
