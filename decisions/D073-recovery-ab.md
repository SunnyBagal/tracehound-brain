---
id: D073
title: Pre-registered dev A/B of the two generation-recovery switches (with 073a)
date: 2026-11-08
kind: experiment
lane: benchmark
outcome: tie
call: flawed
judged_by: bunny (backfill, from the repo record)
judged_on: 2026-11-11
repo_entry: https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#073--pre-registered-dev-ab-of-the-two-generation-recovery-switches-2026-11-08
pr: none (branch bench/recovery-ab, merged as fd36552)
---

# D073 · Recovery A/B (lenient parser, fresh retry)

## Decided
A paired dev A/B of the two default-off switches from #67, on the 40 dev PRs:
- **A:** both off (the product);
- **B1:** `parser: "lenient"`;
- **B2:** lenient + `retryOnLength: "fresh"`.

How it ran:
- **One process, one shared response corpus**, which replays the cache by exact key; no folder scan.
- **Cap:** $4 job-wide.
- **Gate:** devin's E3 replaced the research run's version, by the owner's choice.

## Why, at the time
- The [7 Nov research run](../research/R17-multi-agent-review-2026-11-07.md) named this as the one next task: the only measured delivery defect with a fix already built.
- On 3.5 dev, 7 of 40 PRs per run lost a reviewer chunk and 3 of 40 ended empty ([D068](D068-partial-reviews.md)).

## Expected
Pre-registered exit criteria; an arm passes only if all hold:
- **E2:** terminal failed chunks ≤ 2, strictly below A, and 0 PRs left empty;
- **E3:** TP ≥ A and P ≥ A under both judges;
- **E4:** strict-valid replies parse identically under lenient.

Any unpaired pull or incomplete coverage makes the result inconclusive.

## What happened
Source: [docs/bench/recovery-ab.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/bench/recovery-ab.md).
- **The first run was void.** Live 3.5 calls failed at exactly 300 s: Node's fetch cut them off below the client's own 600 s timeout ([300 s cutoff](../project/300s-transport-cutoff.md)). It spent $0.34415, and the run was restarted once.
- **The restart:**
  - E2: terminal failed chunks A 1, B1 0, B2 7;
  - E3: v1 TP 32 → 33 for B1;
  - E4: 0 of 96 replies differ.
- **Verdict by the rule:** B1 passes, and B2 fails because its fresh retries turned recoverable chunks into failures.
- **Spend:** $0.73195 for the job.
- **073a found the flaw in the setup.** The baseline lost 1 chunk where runs 7r and 8 had each lost 8. Empty replies are never cached, so a replay corpus under-represents exactly the failure being tested ([cache survivorship](../benchmark/cache-survivorship.md)).
- **B1 was relabelled** "harmless, benefit unproven". The lenient parser became the default, with no reliability gain claimed.
- **Standing rule added:** reliability tests bypass the cache.

## Verdict
- **Outcome:** tie. The default changed with no measured benefit and no harm, and the fresh retry was rejected.
- **Call:** flawed. The design could not measure its own target, because the empty replies that cause most losses never reach the cache. The repo's own audit (073a) says so.
- **What we'd do differently:** draw the requests under test fresh, with the cache neither read nor written, as the standing rule now requires. Check a replay baseline's failure count against the original runs before trusting it.
- **What it rules out:** `retryOnLength: "fresh"` (rejected), and the transport as a cause of October's chunk losses (0 fetch errors in runs 7r/8/9).

## Links
[empty replies](../project/empty-replies.md) · [D074](D074-hard-chunks.md) · [R17](../research/R17-multi-agent-review-2026-11-07.md)
