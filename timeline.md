# Timeline

From git history on TraceHound main (merge dates) and decisions made in chat. PR detail: [prs](prs.md). Decision detail: [digest](decisions/digest.md).

## Phase 1: graph ([history](history/phase-1-graph.md))

- **2026-09-30** First commit in TraceHound's history: pnpm workspace, CLAUDE.md, the decision log. Decisions 001–031 appear the same day, likely carried over from earlier work. Viewer canvas polish (#1). Repair agent v2 fails the toy gate at 2/5.
- **2026-10-01** Analyzer 0.7.0 with BullMQ (035). Docker becomes the sandbox of record after Nebius Sandboxes turn out to be unavailable (036). Naming audit finds most summaries wrong. Canvas polish 2 (#2).
- **2026-10-02** Change sets (038), multi-repo index and version guard (039); Recall published as the second repo.
- **2026-10-03** Change view (#3). Repair harness on Recall (#4, 041). BullMQ through DI, analyzer 0.10.0 (#5, 042). Per-test gate and study S1: 2.1% pairing on unseen repos (#7, 043). Build to the evaluation freeze (#8, 044–046).

## Phase 2: repair evaluation ([history](history/phase-2-repair-eval.md))

- **2026-10-04** agent-v6 and re-freeze (#9, 047). Reproduce stage (#10, 048). Wolf logo, canvas polish 3 (#11, #12).
- **2026-10-05/06** Held-out evaluation runs: 160 counted runs, $5.72.
- **2026-10-06** Protocol amendment 1 (#13, 049). **Result: the graph did not help** (#14, #15, [D050](decisions/D050-graph-did-not-help.md)).

## Phase 3: finder ([history](history/phase-3-finder.md))

- **2026-10-06** Finder rules (#16, 051) and finder claims (#17, 052). Railway restyle (#18).
- **2026-10-07** Nebius feedback backfill (#19). Landing page (#20). **Pivot: TraceHound becomes a PR reviewer** ([P001](decisions/P001-pivot-to-reviewer.md)).

## Phase 4: reviewer ([history](history/phase-4-reviewer.md))

- **2026-10-14** Disclosure of who saw Recall's bugs (#21, [D053](decisions/D053-recall-disclosure.md)). Codex and Grok lanes set up ([P002](decisions/P002-lanes-and-tools.md)). Relay per-run Nebius cap set to $3.
- **2026-10-15** Scorer and judge (#22, [D054](decisions/D054-review-benchmark-scorer.md)). Diff-only baseline, F 32.2% (#23, [D055](decisions/D055-diff-only-baseline.md)). Verifier v1 loss (#24, [D056](decisions/D056-verify-v1.md)). Grok's first audit passes the kill test ([P003](decisions/P003-grok-kill-test.md)). GitHub Action (#25). Repo context ties, F 31.8% (#26, [D057](decisions/D057-repo-context.md)). Context verifier F 32.8%, below the 35.0% bar, but it separates TPs from FPs (#27, [D058](decisions/D058-verify-ctx.md)). This vault started.
- **2026-10-16** Action fixes, Codex review rules, lockfile fix (#28). Sunny picks pooling before scorer fairness. 059 pool-verify-v1: **F 35.6%, first variant over a pre-set bar, adopted**. The raw pool reaches 51.4% recall (PR #29, [D059](decisions/D059-pool-verify.md)). Codex switches the Action to pool-verify-v1 (#30). 060: a Qwen judge confirms the ranking (pool-verify-v1 5th of 11 under both judges; F 44.6 under Qwen); the validity check is too lenient to use (#31, [D060](decisions/D060-scorer-checks.md)). Grok research R1–R3 filed, all citations real.
- **2026-10-17** All 7 Action runs fail on pnpm setup (codex-05 written). Grok G1 audit passes, G2 video script filed. UI lane set up (ui-01 + addendum). Product backlog added to the roadmap. 061 Cal.com test plan and runner (#33, $0); Sunny asks for one restart for a crashed run (061a) before merge; 061a done (#33 head `81d0147`). codex-05 (#34) fixes pnpm: all 7 Action runs succeed; demo catches the seeded bug in 2 of 4 PRs, 0 comments on the control. Grok asked for the 19 Oct X post. #33 and #34 merged (merge commits). `review-freeze-1` tagged at 22:14; 062 running.
- **2026-10-18** UI round 1 (#35) checked and merged. **062 blind Cal.com run: pool-verify-v1 F 58.3%, 3rd of 6** (behind Augment, Cursor; ahead of Greptile, Copilot, CodeRabbit), prediction (b) wrong; dev spread 35.6 → 48.0; $5.31 (#36, checked). #36, #32 (dogfood on) and #37 (Super summaries) merged. 063 results page (#39). Fix suggestions are live: demo #3 shows a working "Commit suggestion" button (#38).
- **2026-10-19** ur-01 fixed all 10 Ultrareview findings on #43 (one deferred to backlog). The chat re-checked the source diff and re-ran the tests (153/153). #44 merged (`c6c12b9`), then #43 merged (`2af8c3a`). **v2 architecture proposed** ([P007](decisions/P007-v2-architecture.md)): gate hard, cross-file contracts, learning. Prompts codex-08 (gate), cc-03 (contracts) and grok-07 written. Decided: no standalone 065, and Ultrareview #2 moves to int-01.
- **2026-10-19 (night)** 064 vote across runs: a **loss** (#46). The 5-run spread of pool-verify-v1 is 35.6–48.0 F, so comparisons now use means. cc-03 contracts (#47): 10 of 10 fixture breaks caught, 0 findings on the controls. codex-08 gate (#45) is on hold: its nit rules drop real bugs (fix codex-08b). 065 (evidence verifier + gate) written.
- **2026-10-19 (day)** #46 and #47 merged. 065 run 1 stopped at step 0 ($0). grok-07 → R8 (links recovered, 5 of 5 checked). **Sunny keeps CodeRabbit** in tests and numbers. cc-04 seeded test written. codex-08b fixes #45 (re-checked). Handoff to a new chat at 13:45.
- **2026-10-19 (afternoon)** cc-04 seeded test built (30 branches; scoring fix cc-04b sent). **065 evidence verifier + gate: a loss** (#48). Chat proposes stopping precision tuning (P007 update). 066 results-docs and codex-09 learning written.
- **2026-10-20** rb-01 → #50 (pre-3.5 prompt; rb-01b needed). grok-08 → [R11](research/R11-grok08.md): no new ReviewBench candidate.
- **2026-10-20 (evening)** #50 (ReviewBench image + rb-01b), #49 (learning, after codex-09b) and #51 (missed-issue pass, candidate 6) merged. P008 amendments 2–4. Nemotron 3.5 Super VL is listed but returns 404 (067 stopped at $0). Bunny, the second Claude Pro account, does 066 (#52). Grok bots: R12 (portal; the test-run judge is self-funded), R13 (tool terms), R14 (Devpost checklist; no competitor trademarks in the video). Handoff Oct20b.
- **2026-10-20 (night)** #52 merged (066 results docs). int-01 → #53 (contracts in the Action; chat check passed, Ultrareview #2 next). bunny-02 → #54 (judges' README, stays a draft). Found: the demo and dogfood workflows run Nano generators, not the benchmarked Super.
- **2026-10-21** Ultrareview #2 on #53: 7 findings, all confirmed; codex-11 fixed them (`3b196fa`), #53 cleared. Smoke runs 1–3 on 3 Super are green on image `ff22b972`. ReviewBench registered (onboarding PR #92, candidates 1–2; the judge is paid by ReviewBench). 3.5 found on us-north1. Codex extended (ChatGPT Plus).
- **2026-10-22/23** 067b: 3.5 adopted on 2 dev runs (#56 merged, `8d0fb7a`). The chat caught a bench-vs-product mismatch on failed chunks → codex-12 / decision 068 (#57: 3.5 default + partial reviews), cleared. Handoff Oct23.
- **2026-10-23** #57 merged (`0aeef06`). ReviewBench onboarding #92 merged (approved 22 Oct). Freeze-2 pulled forward to 23 Oct because of HackerRank Orchestrate (24–25 Oct). Top-up to $70.69. P008 amendment 5 approved and written (all candidates on image `9b9a5127`, partial results). Smokes on pool-raw/3.5, pool-verify-v1/3.5 (first `ok partial=1/2` live) and pool-raw-x2 all green. **`review-freeze-2` → `0aeef06`**; dogfood (`8712273`) and demo (`88f6721`) re-pinned with `model:` 3.5. Demo #2 on 3.5: no findings, nothing posted (no change made). 069 v2 blind Cal.com run done the same evening (#59): v1 F 57.1 / P 63.2 on Cal.com (3 Super run 1: 58.3 / 56.0), all three predictions right; all-50 v2 is 3rd of 6 under both judges as a point estimate. bunny-04 Devpost drafts (#58). codex-13 stopped on test-timeout flakes → 13b.

## Ahead

19–23 Oct v2 stages and integration → **24 Oct freeze-2** → 25 Oct one blind Cal.com run of v2 plus the seeded test → 26 Oct results and the judges' README → 27–28 Oct Devpost and video → 29 Oct submit. See [roadmap](project/roadmap.md).
