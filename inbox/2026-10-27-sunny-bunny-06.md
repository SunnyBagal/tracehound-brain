# bunny-06 report: final fill of README #54 and Devpost #58 from numbers.md

Docs only, no model calls, $0. Neither PR merged; both still drafts. No tags, no rebase, no force-push.

## Step 0
- `git fetch origin`: ok (main 5fb8cdc → 36de717). `git status --porcelain`: empty.
- GIT_* env check: printed nothing.
- Brain `git pull`: already up to date.
- Read: submission/numbers.md (all), bunny-04 out (12 [[SUNNY]] questions), both PR branches, docs/bench/v2-calcom.md, v2-intervals.md, reviewbench.md, v3-recall.md (head). All checks passed.

## Heads
- **#54** `docs/judges-readme`: `64082a7a29ee21c4c3e2ebd89792b8c2fc9f1cce` (merge of origin/main `760cc93`, clean, then one commit `readme: …`).
- **#58** `docs/devpost-drafts`: `8184e54a36516503605230ed1030c585ba880027` (merge of origin/main `2b63b08`, clean, then one commit `docs(devpost): …`).

## Part A: README (#54), placeholders filled
- `[[RESULTS-TABLE]]` → §1 table (blind Cal.com, both judges, v1 row kept) + §1 allowed sentence with names; §2 table + §2 allowed sentence (names in parentheses) + v2 vs v1 by judge (+11.0 [+1.8, +20.1] under v1, tied under x1). Rule 6 note: 40 of 50 are the tuned-on dev set.
- `[[CALCOM-V2]]` → one paragraph: v2 is the product config, second blind look, chosen on dev, pre-registered before the first 3.5 call, nothing tuned after, no second v2 run, v1 row kept beside it.
- `[[REVIEWBENCH]]` → §4: six-run table, "test runs, not a leaderboard submission", no final (~$107 with VAT vs $43.52, measured $3.45 per round), selection bias, "3.5 won on our dev set but lost on ReviewBench" (12.2 vs 13.9; 13.5 vs 16.0).
- `[[COST]]` → §8 (+ §3's median/mean and the ~7 of 40 invalid-JSON line).
- `[[SEEDED-TEST]]` → `[[TBD-SEEDED]]`.
- Added `## Losses` (§6, plus the §5 Ultra allowed sentence and run 10's pool-raw interval; run 11 `[[TBD-ULTRA]]`) and `## Live demo` (§7: pinned @0aeef06 with 3.5; PR #2 posted no findings under 3.5 on 24 Oct; older reviews were Nano generators; seeded PRs `[[TBD-SEEDED]]`).
- `## What's next` existed: replaced with §9's four items (two carry `[[TBD-LEVERS]]`) and kept the existing learn-measurement item.
- Stale fixes: install block pins `@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2` with `model: 'nvidia/Nemotron-3.5-Super-VL'` (fixes stay on 3 Super, per docs/action/README.md). "Built with" says 3.5 Super is the product model (decision 068).
- Contracts bullet gets §0 rule 10 verbatim: "no model calls; network only for a credential-free git fetch of the base commit".
- main's `## PR reviewer benchmark` section (v1, "4th of 6") kept unchanged, with one lead sentence that labels it the first look (3 Super, review-freeze-1) and points to Results.

## Part B: Devpost (#58)
- writeup.md: header rewritten (open slots + §0 rules). Results block replaced: §1 numbers + allowed sentence without names ("(only 10 PRs)"), §2 numbers + allowed sentence, §4 ReviewBench test runs (product 12.2, pre-registered winner 21.3, no final and why, selection bias), §5 Ultra (report only, allowed sentence, `[[TBD-ULTRA]]`), seeded `[[TBD-SEEDED]]`. Contracts "no model, no network" → rule 10. Losses: 070 and 071 added to the published-losses list, plus v3 hampered / 2 defects documented-not-fixed (credited to "an automated review", not Bugbot by name, rule 4) and no ReviewBench final. "What we learned": the 3.5 dev-vs-ReviewBench finding. Cost line → §8. "68 decisions" → count dropped (not in numbers.md).
- Chat's answers applied: Q2 (§1/§2 allowed sentences, no names), Q3 (assistants named factually in "How we built it"), Q4 (§9 replaces What's next), Q8 (pin in #54 confirmed), Q9 ("Trying it without a key" paragraph: browse recorded reviews, dashboard, seeded PRs; no trigger without write access, because the Action runs on same-repo PRs only and spends our key), Q11 (in the video script's "Before shooting"), Q12 (holds; recorded in the script with the times, SUNNY marker removed).
- video-script.md: column renamed "what's on screen"; shot 6 card and voice-over from §1/§2/§4; shot 7 adds "on ReviewBench test runs, 3.5 lost to 3 Super"; install block pins 0aeef06 with the explicit 3.5 model; shots 2 and 3 marked `[[TBD-SEEDED]]`; timing still 170 s (shot 7 18 s, shot 8 12 s).
- checklist.md: statuses as of 27 Oct, owners from Sunny/chat/relay/Bunny/Cursor/Gemini (from NOW.md's lane table), a slot table for the three TBDs, a 27–29 Oct plan.

## Left open
- `[[TBD-SEEDED]]`: README ×2; writeup ×2 (+ header); video-script shots 2, 3, 6 and "Before shooting"; checklist.
- `[[TBD-ULTRA]]`: README Losses; writeup Results (+ header).
- `[[TBD-LEVERS]]`: README What's next ×2; writeup What's next ×2 (+ header).
- `[[CHAT: …]]`: none.
- `[[SUNNY: …]]`: Q1 personal reason (writeup Inspiration); Q5 YouTube URL (writeup Links, checklist #9); Q6 and Q7 what existed before 26 Aug / the 30 Sep work (what-changed ×3, checklist #14); Q10 Vercel until 15 Dec (checklist #7).

## For chat to check
- Q3 puts "Cursor" in the Devpost text as a coding assistant. Cursor is also one of the five benchmarked reviewers, so a reader could connect them. It's flagged in checklist #11. Drop it if rule 4 should win.
- The 071b inbox note already has run 11 numbers (Ultra pool-verify-v1 run 11 F 51.9, mean 47.1), but numbers.md §5 still says TBD, so I kept `[[TBD-ULTRA]]`.
- writeup.md still has a few older numbers from bunny-04 that aren't in numbers.md (e.g. 79 of 624 calls at the token limit, 1,305 tests on 23 Oct, $0.0113 ledger gap, 26 FEEDBACK entries). They came from repo docs and weren't placeholders, so I left them. Gemini's claims check should cover them.

## Gates (before each commit, plain shell)
- GIT_* check: printed nothing (both times).
- `pnpm test`: action 237 passed; analyzer 946 passed, 3 skipped; viewer 188 passed (both branches). No timeouts, no reruns.
- `pnpm typecheck`: green (both branches).
