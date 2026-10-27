# fill-01 report (relay, Sunny's Claude Code, account A)

## Step 0
- origin/main = `5ba60a0`; tree clean; GIT_* env check printed nothing; `pnpm install --frozen-lockfile` OK.
- `pnpm test`: action 237 passed, analyzer 996 passed + 2 skipped, viewer 188 passed → **1,421 passed, 2 skipped**. No timeouts, so no reruns. `pnpm typecheck`: 3/3 packages green.
- One thing to know: my local brain clone was behind origin. Its numbers.md still had `[[TBD]]` in §5 and §9. I ran `git pull --rebase` (66414f9) before cross-checking, and the pulled numbers.md matches the brief. No STOP.

## Part 1: PR #66 `bench docs: results.md and /benchmark on v2 (fill-01)`
- Branch `docs/fill-results`, head **`6c45378`**, marked ready. **CI: all green** (test-and-build, review, Vercel). No review comments yet. Not merged.
- Commits: 12e01ef brief · 292a75a results.md v2 · cbda2b7 reviewbench.md test runs · efbd42b v3-recall fixes + defects · bb30106 benchmark.ts tables · 127470b pin 292a75a · b380d2c README anchors · b0f1bc4 reword "the best of them carries selection bias" → "the top score among them…" (results.md, reviewbench.md) · 40e0b41 re-pin · 6c45378 README v1 link pinned.
- Tables in results.md / benchmark.ts: **9**, not the brief's 8. The extra one is table 9, the dev cost per-PR table. The brief asked for dev-3.5.md's per-PR table in the Cost section, and the viewer test checks every table in the file, so it is transcribed too.
  1. Blind test: Cal.com, v2: 9 rows
  2. Cal.com: v2 minus each reviewer, 95% intervals: 6 rows
  3. All 50 PRs, bench-judge-v1 (scorer of record): 8 rows
  4. All 50 PRs, bench-judge-x1 (report only): 8 rows
  5. Dev set (40 PRs, n = 74), mean per model: 4 rows
  6. ReviewBench test runs (25 PRs, one round, ReviewBench's own judge): 6 rows
  7. v1 history: every Cal.com run (3 Super): 3 rows (unchanged)
  8. Precision levers tried on dev: 12 rows (6 unchanged + 067, 070, 071, levers v2 cap / verifier on-off / 2-of-3 vote)
  9. Dev cost per pull request, pipeline only: 3 rows
- Source vs numbers.md disagreements: **none**.
  - Between two sources (not numbers.md): v3-recall.md gives 3 Super pool-raw x1 as a "mean of 4" (48.7), with run 1 not scored. dev-ultra.md and levers-v2.md have run 1 scored, so the mean of 5 is 47.0. Table 5 uses dev-ultra's 47.0, as the brief says.
  - The $57.05 project spend and the $43.52 balance have no repo source. results.md cites them as the owner's record / P008 amendment 8.
- "best" on the page: the only occurrence is the unchanged 064 row ("best variant: vote-k3of5"), which the brief requires to stay as it is. "3rd of 6" is not in benchmark.ts. "all 50 PRs" appears only in the titles of tables 3 and 4.
- Pinned revision: `BENCHMARK_REVISION` = **`b0f1bc4f32817e0433026ad093a76a77a04599fb`** (the last commit that changed results.md).
- Anchor links changed (README.md:24 on main):
  - `#6-precision-levers-tried-on-dev` → `#7-precision-levers-tried-on-dev`.
  - `#5-all-50-prs` → `https://github.com/SunnyBagal/TraceHound/blob/e91a268/docs/bench/results.md#5-all-50-prs` (text "v1 all-50 table"). That paragraph is v1 text ("4th of 6"), so I pinned it to v1's page rather than pointing it at the v2 table.
  - Nothing else outside decisions.md links a results.md anchor.
- Gates on the final tree, each run separately: viewer test 188/188, viewer typecheck OK, viewer build OK, `pnpm test` **1,421 passed, 2 skipped**, `pnpm typecheck` OK.
- Expected, not a defect: at commits 292a75a–efbd42b the viewer table-copy test fails, because results.md changes before benchmark.ts. It passes from bb30106 on.
- v3-recall.md units:
  - "15 request errors" = per failure string (9 lens calls + the missed passes of 6 PRs).
  - Lens failures are counted per lens call. Missed failures are counted per pull request; each arm's list includes 1 PR whose pass was OK with failed chunks (checked in the records: S 5 FAILED + 1, V 22 + 1).
  - Budget refusals are counted per refused request.

## Part 2: PR #54, `docs/judges-readme`
- Head **`2bb3144`** (from 64082a7, no merge needed). Commits: 584edf6 fills · 2bb3144 anchors. Not marked ready.
- `[[TBD-ULTRA]]` + the run-10 sentence → "Over dev runs 10 and 11 (judge v1), its pool-verify-v1 mean F was 47.1, against 3.5's 50.4 and 3 Super's 42.3, with no interval-backed difference. Its pool-raw mean F was 43.6 against 3.5's 53.9 (−10.2 [−17.7, −3.2]), the only interval that excludes 0. Under judge x1: pool-verify-v1 54.9, pool-raw 51.4 ([docs/bench/dev-ultra.md](docs/bench/dev-ultra.md))." It stays inside the existing "report only, free NVIDIA API, not Token Factory, not the product" paragraph.
- `[[TBD-LEVERS]]` ×2 and the "verifier doesn't pay" bullet → four bullets:
  - "A per-PR comment cap, against product noise: replayed on saved dev runs ($0, [docs/bench/levers-v2.md](docs/bench/levers-v2.md)), no cap on the shipped, verified pipeline improves F with an interval excluding 0; small gains appear only on the unverified pool at k = 4–5. A hypothesis, not adopted."
  - "The verifier: on dev, verified scores above raw with an interval excluding 0 in 2 of 9 runs (3 Super 2r, Ultra 11); on 3.5 neither run shows a difference (−1.6 and −5.4, intervals include 0); on ReviewBench raw scores higher in both models. An open question, not a finding ([levers v2](docs/bench/levers-v2.md))."
  - "A cheaper 2-of-3 vote across runs: no gain established (levers v2)."
  - "Where the misses are: both 3.5 dev runs miss 35 of 74 Medium+ golden comments; none of the 7 Critical ones is missed by both; the most missed repo is sentry (14 of 21 Medium+)."
  - "beats" from numbers.md §9 is written "scores above" (§0 rule 1).
- Anchors:
  - `#6-precision-levers…` → `#7-…` (2×).
  - The v1 section's `#5-all-50-prs` → pinned `e91a268` link (same as on main, so a later merge is clean).
  - The `#7` anchor resolves once #66 is merged.
- grep TBD-ULTRA/TBD-LEVERS: nothing. TBD-SEEDED: 2 (unchanged). `[[SUNNY`: 0.

## Part 3: PR #58, `docs/devpost-drafts`
- Head **`e596309`** (from 8184e54, no merge). Commits: 5d9d2aa fills · e596309 claims fixes. Not marked ready.
- writeup.md fills (no names):
  - The two open-slot bullets for TBD-ULTRA and TBD-LEVERS are removed.
  - Ultra → "Over 2 dev runs (judge v1), its pool-verify-v1 mean F was 47.1, against 3.5 Super's 50.4 and 3 Super's 42.3, with no interval-backed difference. Its unverified-pool (pool-raw) mean F was 43.6 against 3.5's 53.9 (−10.2 [−17.7, −3.2]), the only interval that excludes 0. Under the second judge: 54.9 and 51.4."
  - What's next:
    - "A per-PR comment cap, against noise in posted reviews: replayed on saved dev runs at $0, no cap on the shipped, verified pipeline improves F with an interval excluding 0; small gains appear only on the unverified pool at k = 4–5. A hypothesis, not adopted."
    - "The verifier: on our 40-PR dev set, verified scores above the unverified pool with an interval excluding 0 in 2 of 9 runs; on 3.5 neither run shows a difference (−1.6 and −5.4, intervals include 0); on ReviewBench the unverified pool scores higher in both models. An open question, not a finding (replay: `docs/bench/levers-v2.md`)."
    - The 2-of-3 vote and misses bullets, as in the README.
- Claims fixes:
  1. The "verifier doesn't pay" bullet is replaced by the verifier bullet above.
  2. "(1,305 tests on 23 Oct)" → "(1,421 tests on 27 Oct)".
  3. "Claude Code, Codex, Cursor, Devin, Grok and Gemini" → "Claude Code, Codex, Devin, Grok and Gemini".
  4. "26 dated feedback entries" → "29" (`^## 20` in FEEDBACK.md on origin/main = 29).
  5. "a test fails the build if they differ" → "a test fails CI if they differ".
  6. checklist.md:
     - Owners: the chat does the claims check; Gemini and Cursor are removed; UI = **Claude B** (ui-11).
     - Slot rows TBD-ULTRA/TBD-LEVERS: "✅ done: relay (fill-01, 27 Oct)".
     - Rows 5 and 15: open lists updated.
     - Row 11: Cursor left out as a coding tool, per §0 rule 4.
     - Plan rows: UI → Claude B, claims check → chat.
- Remaining counts:
  - writeup.md: TBD-SEEDED 3, SUNNY 3
  - checklist.md: TBD-SEEDED 5, SUNNY 5
  - video-script.md: TBD-SEEDED 4, SUNNY 0
  - All unchanged.
- Checks:
  - `git grep -n -i "cursor|augment|greptile|coderabbit|copilot|bugbot" origin/docs/devpost-drafts -- writeup.md video-script.md`: nothing.
  - TBD-ULTRA/LEVERS in writeup.md: nothing.
  - beats/best/outperforms/state of the art: only writeup.md:13–14, the draft's own rules sentence that lists the banned words (left as is).

## Not done / notes
- Nothing skipped. $0, no model calls. Nothing merged, no tags, no force-push.
- README.md:24 on main still has v1 prose (the README rewrite is #54); I only changed its links.
- viewer/app/benchmark/page.tsx (ui-11's) still has v1-era static text: "the five comparison tools were judged once while TraceHound was run three times", "fourth reviewer variant", "judge is the same model … as the reviewer". I did not touch it. ui-11 or the chat should update it.
