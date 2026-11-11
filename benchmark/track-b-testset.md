# Track B: a fresh TypeScript test set (method and counts only)

> **Public note.** The test set is private (`SunnyBagal/tracehound-testset`). This note has counts and method only: no case ids, PR numbers, shas, labels, file paths or bug descriptions. Cases are split into **dev** and **held-out**. Builders who tune TraceHound never see held-out content. Held-out numbers here are counts only, copied from the repo's `.agents/LOG.md` and `.agents/STATE.md`.

## Why
- Cal.com is spent ([D069](../decisions/D069-v2-calcom.md)), and the old dev PRs are the selection set.
- The [7 Nov research](../research/R17-multi-agent-review-2026-11-07.md) asked for a fresh TS dev + held-out set split by repo, with the list hash committed first.

## Source of bugs (SZZ-style)
- **What a bug case is:** a bug case is a merged pull request whose changed lines a later merged fix repaired. The introducing PR is the case.
- **What a clean case is:** a clean case is a PR whose checked added lines (at least 10 non-trivial lines) no fix-shaped later commit touched, up to the pinned HEAD. It is not a claim that the PR is bug-free: a bug never fixed by then is invisible to this method (method from the test set's pilot write-up, §3).
- **Repos:** medusa, twenty and payload, plus bullmq for held-out only. Each is reviewed on a public SunnyBagal fork.
- **The diff under review:** each case is the commit pair base → case head on the fork, and the case PR targets a per-case base branch, never the default branch.

## Steps and counts
1. **Pilot (2026-11-08):**
   - 13 cases (10 bug, 3 clean) from twenty and medusa;
   - two labellers, strict agreement 10/13;
   - label schema v2 (one primary bug per case) brought agreement to 13/13.
2. **Split before any new labelling (2026-11-09):**
   - **universe:** 32,233 introducing squash PRs across medusa, payload and twenty;
   - **seed:** derived from the hash of the committed list;
   - **stratified** by repo;
   - **pilot cases** pinned to dev.

   Payload was added because twenty + medusa could not reach 80.
3. **Sizes after labelling:**
   - dev 40 (31 bug, 9 clean);
   - held-out 28 (19 bug, 9 clean), 12 bug cases short after every fix pool was used.
4. **Held-out fill (owner's ruling, 2026-11-10):**
   - **rules:** bullmq only, fixes since 2026-07-01, ≤ 400 lines, the same rules, TS only, held-out only, no rule relaxing;
   - **result:** +1 bug case, so held-out is final at **29 (20 bug, 9 clean)**.
5. **Label agreement (numbers only):**
   - dev: 40/40 strict and bug/clean;
   - held-out: 29/29.
6. **Attribution check (does the fix really touch the case's lines?):**
   - **dev:** 30 attributed, 1 moved, 0 not attributed, after replacements;
   - **held-out:** 19 attributed, 1 unmappable.
7. **Dev replacements (owner's ruling, 2026-11-10):**
   - one dev case was dropped as not a bug;
   - cases failing attribution were replaced by the next candidate in screening order that passes every check, else dev shrinks by one;
   - **fallback (b):** when a repo's pool was exhausted, the slot went to another repo.

   That replaced 5 slots on 2026-11-10, and the blind second labels agreed 5/5. Dev stayed 40 (medusa 16, payload 9, twenty 15 as of 2026-11-10), with attribution 30 / 1 moved / 0 not attributed.
   **2026-11-11:** one dev case had been flagged "unclear". It was ruled on and replaced under the same rules plus a new check, "the bug is visible at the introducing PR". Its blind second label is in progress. Dev counts stay **40 (31 bug, 9 clean)**.
8. **Dev clones:** filled by a read-only batched fetch from the forks: 94,091 blobs, and all 80 dev case commits complete.

## Scoring scheme (devin, in [docs/budget.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/budget.md))
- **TS-dev:** one judge (bench-judge-v1). Select only on a margin of at least 3 cases.
- **Held-out:** both judges, then Sunny checks by hand:
  - every match;
  - every disagreement;
  - every comment within ±10 lines of a labelled range.
- **Supported claim:** bugs caught of N labelled bugs, plus a lower bound on precision. It does not support true precision, all-bug recall, or "clean = bug-free".
- **Still open:** goldens per case, golden severity and comments per case must be fixed in writing before the held-out run.

## Firewall (general terms)
- **Who may read held-out:** the agents who tune TraceHound never open, list, grep or diff held-out files, and never read the screen of an agent doing held-out work. Only numbers-only reports reach them. Agents who don't tune may read held-out.
- **Two slips, both disclosed in the log, with no held-out content seen:**
  - one script run that briefly attached held-out objects as a git alternate (counts printed only);
  - one read of a dev agent's screen that showed two excluded candidates (dropped).
- **What changed:** held-out reading in the attribution script now needs an explicit flag. See [lessons](../lessons.md) (2026-11-10).

## Live plan
- **Tools:** CodeRabbit (manual trigger) and Greptile Starter. Cursor Bugbot is excluded.
- **Forks:** Actions is off on all four forks.
- **Timing:** held-out PRs open only after a final freeze tag, with every tool in one window.
- **TraceHound:** runs locally ([local runner](../ops/local-runner.md)).
- **Source:** [docs/live-setup.md](https://github.com/SunnyBagal/TraceHound/blob/main/docs/live-setup.md).
