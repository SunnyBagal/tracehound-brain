---
updated: 2026-11-22
updated_by: artu1 (end of the held-out run; from the repo's .agents/STATE.md and docs/bench/ts-heldout.md at main 890943e)
---

# Now

**The live state is the repo's [`.agents/STATE.md`](https://github.com/SunnyBagal/TraceHound/blob/main/.agents/STATE.md)** (rewritten every session); history is in [`.agents/LOG.md`](https://github.com/SunnyBagal/TraceHound/blob/main/.agents/LOG.md). If they disagree with this page, they win.

## 22 Nov: the held-out result is in and published
- **Question (registered before the window, decision 078):** on fresh TypeScript pull requests, how many real bugs does TraceHound's frozen reviewer (A0: pool-verify-v1 on Nemotron 3.5 Super, tag `review-freeze-3`) catch next to CodeRabbit and Greptile? A bug is one that a later merged fix repaired.
- **Set:** 29 pull requests opened on our forks of medusa (8), payload (10), twenty (10) and bullmq (1): 20 introduced a labelled bug, 9 are clean. All three tools reviewed all 29 in one 72-hour window (19–22 Nov).
- **Result (hand-checked, blind to the tool; Wilson 95%):**

| tool | bugs caught of 20 | comments per PR | comments on the 9 clean PRs | cost per PR |
|---|---|---|---|---|
| TraceHound A0 | 4 [8.1–41.6%] | 0.76 | 2 | $0.0603 (ledger) |
| CodeRabbit | 10 [29.9–70.1%] | 2.83 | 15 | free tier |
| Greptile | 12 [38.7–78.1%] | 2.72 | 13 | free tier |

- **Registered tests (exact sign test, two-sided, 0.05, uncorrected):** TraceHound vs CodeRabbit 2 vs 8, p = 0.109 (no detectable difference at n = 20); TraceHound vs Greptile 0 vs 8, p = 0.0078 (Greptile caught more).
- **Product rule (decision 079, registered before T0):** `D_H` = −6 and −8, worse pair −8 → **band D: stop reviewer work; publish the result and the method.** This agrees with decision 080. No paid tier.
- **Predictions (078):** 8 right, 6 wrong, under a scoring rule chosen after the result and labelled so (decision 081 item 4). The biggest miss: TraceHound was predicted to catch 5–11 and caught 4.
- **Haiku 5.5 (report-only):** two draws, 6 and 7 catches under the first judge (A0: 6). The condition for a default switch was met; nothing was switched (080).
- **Spend:** Nebius ledger $1.82 for the held-out run (A0 reviews $1.75, H2 judging $0.08), plus $0.37 on Anthropic for the two Haiku draws ($0.1864 + $0.1864). The hand-check took Sunny one sitting of 3 h 07 min over 61 items.

## Where it is written
- Page: [docs/bench/ts-heldout.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/ts-heldout.md); summary in the repo README and on the live [/benchmark](https://tracehound-tau.vercel.app/benchmark) page.
- Decision: [081](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md) (band D, scoring rule, Haiku not switched).

## Promo video for the result post
- The MP4 is `promo/race/renders/tracehound-race.mp4` on TraceHound branch `promo/race-animation` ([PR #79](https://github.com/SunnyBagal/TraceHound/pull/79), not merged), with stills `tracehound-race-first.png` and `tracehound-race-scoreboard.png` beside it. It uses only the published totals.

## What happens now
- **Maintenance only (decision 080):** fixes, no new arms, no tuning, no model switches. Company work stays paused.
- Kill dates in the product note: the two that apply to band D are met (score files hashed and band computed by 25 Nov; write-up public by 30 Nov). The band A and B rows do not apply.
- Viewer fix PR #77 (maintenance) merged 22 Nov (`bdc7ed4`). The 50-stars goal by 31 Dec stands as written.

## Team
artu1, bunny, codex, grok (devin out of quota), in Herdr ([P010](decisions/P010-herdr-team.md)). codex went into an error state on 22 Nov at about 04:15Z; grok stood in for it on the snapshot, H2 and every review since.

Earlier versions of this page (11–21 Nov): [archive](history/now-2026-11-11-to-21.md).
