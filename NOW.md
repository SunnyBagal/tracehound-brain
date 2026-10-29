---
updated: 2026-10-30 00:15 IST
updated_by: chat
---

# Now

**Latest handoff: [history/TraceHound_Handoff_Oct30.md](history/TraceHound_Handoff_Oct30.md).** Verify against GitHub before acting on anything here: `git ls-remote https://github.com/SunnyBagal/TraceHound`, and WebFetch of `https://github.com/SunnyBagal/TraceHound/pull/N` for PR state and comments.

**Schedule change (23 Oct 16:40):** Sunny is in **HackerRank Orchestrate** (24h, 24 Oct 18:00 → 25 Oct 18:00 IST). He's unavailable then.
- **Freeze-2 must finish before 24 Oct 18:00**, or move to 26 Oct.
- The v2 Cal.com run (relay, unattended) can run during the event if it's launched before 18:00 on the 24th.
- **The seeded test moves to 26 Oct** (it needs Sunny).
- 26–27: results + README; 27–28: Devpost + video; submit 29 Oct. No buffer is left.
- **Token Factory console balance: $20.69** (23 Oct), with a top-up of about $50 planned → about $70. Expected need: ReviewBench test runs ~$8, final ~$20–41, Cal.com v2 run ~$4, CI ~$2. That's tight; choose the final candidate with cost in mind.

## 30 Oct 00:15: Recall orphans removed (Recall `256513a`), snapshot regenerated (main `b498647`); handoff Oct30
- First regen ran without the key (heuristic names, a viewer test failed, pushed `c451ace` anyway: chat's paste block put the commit on its own line). Fixed by `node --env-file=.env …` → 4/4 named by Super, 0 warnings, 214/214, `b498647`.
- Gallery image 1 padded to 3:2. Form answers and AI-tools answer given. [Handoff Oct30](history/TraceHound_Handoff_Oct30.md).

## 29 Oct 22:20: #69 merged (main `8fbac41`); Devpost form answers; handoff Oct29
- [devpost-form-answers](submission/devpost-form-answers.md): every form field rewritten from numbers.md (old drafts said "N/A", "Good", bare "10", and an outdated model answer). Demo URL field was empty.
- Left: video → YouTube public → Devpost fields → submit; chat PR for `docs/devpost/writeup.md`. [Handoff Oct29](history/TraceHound_Handoff_Oct29.md).

## 29 Oct 22:05: #69 green at `cd7fa11`; video script v3
- #69 now: Q6/Q7; one flag `BLUR_THIRD_PARTY_MARKS` (viewer/lib/marks.ts) blurs hero marks, benchmark reviewer logos (via `.bench-mark-wrap`) and graph tech logos, and shows the five reviewers as **Reviewer A–E** on /benchmark (`lib/benchmark-display.ts`; results.md and benchmark.ts keep the names); Limits under Cost, left-aligned; wolves cut-out (rembg isnet, `viewer/public/wolves-cutout.webp`, credited as owner-supplied) standing on the footer hairline. Viewer 214/214. CI green. Merge (commit).
- Host (Jasmine) only quoted the rule; Izza: blur logos, names asked to team, no answer → Sunny chose A–E.
- [video-script-v3](submission/video-script-v3.md): whole site filmable; never say a competitor name aloud.
- Left: record → YouTube public → URL → chat replaces `docs/devpost/writeup.md` with the final About + URL → submit.

## 29 Oct 20:15: host (Izza, Discord) says blur logos; #69 `bb782ed` green with benchmark logos blurred too
- Host: official rules restrict third-party trademarks in demo videos without permission; blur logos; she's checking with the team on names. Sunny asked (DM, with the all-50 table screenshot) whether names can stay.
- #69 now also blurs the benchmark's reviewer logos via a `.bench-mark-wrap` wrapper (a filter on the masked span itself doesn't show). All behind `BLUR_THIRD_PARTY_MARKS`. Viewer 213/213. CI green. Merge (commit), then record.
- If names are also refused: add a names switch on the same flag, or record the benchmark top section only.

## 29 Oct 19:50: PR #69 green (`79eb40c`): Q6/Q7 + hero/graph mark blur behind `BLUR_THIRD_PARTY_MARKS`
- First CI attempt on `3773465` failed in 28 s (infra; rerun green). #69 now: what-changed Q6/Q7; `viewer/lib/marks.ts` flag (true) blurs hero agent marks (10px) and graph tech logos (5px; masked ones barely change); benchmark reviewer logos not blurred (a filter can't soften a CSS mask; owner's call). Test `blur-marks.test.ts`. CI green. Merge (commit).
- Video: Sunny asked the hackathon host (Discord DM) whether the benchmark page's competitor names/logos may be shown. Until answered: film the benchmark top section only. If "blur": wrap `ReviewerMark`'s masked span and blur the wrapper.

## 29 Oct 19:20: Q6/Q7 confirmed by Sunny; chat opened PR #69
- Sunny confirmed: no code before 26 Aug; spec not public; nothing carried over; 30 Sep commits written that day. Chat (push access added this session) filled the 3 SUNNY slots in `docs/devpost/what-changed-since-26-aug.md` on branch `docs/what-changed-q6-q7` (`3773465`), **PR #69**, docs only. Sunny merges (commit).

## 29 Oct 19:10: final About text, video script v2, Q6/Q7 draft
- [submission/devpost-about.md](submission/devpost-about.md): Codex's About + graph numbers (050), why 3.5 (+8.1 [+1.4, +14.4], x1 includes 0), all-50 sentence, evidence links; Built with without `bun`.
- [submission/video-script-v2.md](submission/video-script-v2.md): Sunny's flow (home → reviewer → benchmark → graph PR lane → sidebar evidence), 2:50. Chat: **blur hero logos and /benchmark names/logos** (Devpost: no third-party trademarks without permission; a disclaimer isn't permission). Disclaimer card reworded: "the reviewer runs only on Nemotron" (Claude Code etc. were used to build it, and the second judge is Qwen, so "none of them are used" would be false).
- [submission/q6-q7-draft.md](submission/q6-q7-draft.md): drafted from Sunny's own past statements (FixFlow idea, FixFlow V2 spec in Aug, no code before 30 Sep, built with Claude Code). **Sunny confirms 4 facts** before it goes in.

## 29 Oct 18:50: main `4843cf2` (#54, #58, #65 merged); codex-20 audit; Devpost About checked
- codex-20 (report not in the brain; pasted in chat): merges, names, pins, links, live site, public repos, About text/topics, clean-clone gates (1,465 + 8 skipped) pass. Must fix: Q5 YouTube URL; Q6/Q7 in `docs/devpost/what-changed-since-26-aug.md` (linked from the writeup, judges will open it; also the "significantly updated" eligibility answer). Numbers "FAIL" = writeup figures sourced from repo docs (60,000 chars, 32,000 tokens, 160 runs …), not numbers.md: not blockers.
- Devpost "About the project" (Codex's text, pasted in the form): chat found no wrong number or forbidden name. Additions: the all-50 sentence (§2), the graph result's numbers (050), why 3.5 with its interval (§3/067), evidence links (results.md, seeds RESULTS.md, decisions.md), and drop the `bun` Built-with tag (TraceHound isn't built with Bun; only the Recall demo fork is).
- After submit: replace `docs/devpost/writeup.md` with the final pasted text (its SUNNY slots are visible in the repo).

## 29 Oct 03:05: submission day plan; Claude at weekly limit until 18:30 → Codex jobs
- [codex-day-29oct](prompts/2026-10-29/codex-day-29oct.md): **codex-19** ui-11c on #65 (in `~/Projects/TraceHound-ui`); **codex-21** Sunny's answers → Devpost slots (from `inbox/2026-10-29-sunny-answers.md`); **codex-20** pre-flight audit of merged main + live site + public repos, and sets the repo About text/topics; **codex-22** optional $0 truncation scan.
- Day: merge #54/#58 → Sunny writes answers → codex-21 → merge #65 (after codex-19 + CI) → codex-20 → record video (script on main), upload to YouTube (public) → fill the Devpost form from writeup.md, save as draft → 18:30+ chat's final check → **submit tonight**. 30 Oct = buffer only.

## 29 Oct 02:45: Sunny keeps the hero logos (incl. Cursor); ui-11c revised
- Sunny's call: the landing page keeps all marks. Chat: OK for the website (nominative use: the agents whose PRs it reviews), with a "trademarks of their owners, no affiliation or endorsement" line under the hero and in CREDITS.md. Unused SVGs stay. The hard rules are unchanged: **no competitor names in Devpost text or video, and the hero is not filmed.** ui-11c now = trademark line + `recallSeeded: true`.

## 29 Oct 02:40: codex-18b done → #54 `7d7b051`, #58 `a14e3c0` OK to merge; #65 needs ui-11c first
- **HOLD on #54/#58 lifted.** Chat checked 18b: shot 6 = `8184e54`'s row + one seeded line (57.1% back), shot 7 → §7, known-limit bullet in both What's next, 0 TBD-SEEDED, names grep empty, timings 2:50. Merge both (commit). Open: SUNNY Q1, Q6, Q7, Q10 now; Q5 after upload.
- **#65** (`690c6b4`): merge onto main clean; viewer 212/212, typecheck green; build blocked only by Google Fonts in chat's sandbox. Benchmark page now looks tables up by kind (ui-11b done). New deps radix-ui, class-variance-authority, `cn` (shadcn's, verified on npm), tw-animate-css: acceptable. **Problems:** Hero `MARKS` includes a **Cursor** logo (benchmarked competitor; violates the landing rule); unused competitor SVGs (coderabbit, augment, greptile, copilot) ship in `public/icons/brands/`; `recallSeeded` flag still false. → [ui-11c](prompts/2026-10-29/ui-11c-before-merge.md). Merge #65 after it + CI green.
- Video: don't film the hero (third-party marks; Devpost video rule).

## 28 Oct 22:35: codex-18 done; chat claims pass on #54 `6c6fb95` / #58 `32833ab`: one real problem → codex-18b
- README seeded section, losses line, Devpost seeded text and "Trying it without a key" match numbers.md §7 verbatim; no names in writeup/video; 0 TBD-SEEDED.
- **Problem:** video shot 6 was replaced wholesale by the seeded sentence, so the video lost the blind Cal.com headline (57.1%, tied with 4 of 5). Cause: chat's brief said "Shot 6 or the closing card gets the allowed sentence". → [codex-18b](prompts/2026-10-28/codex-18b-video-shot6-and-limit.md): restore `8184e54`'s shot 6 with a one-line seeded result; shot 7 §6 → §7; add the devin-01 known-limit bullet to What's next (README + Devpost).
- After 18b: remaining = Sunny's 5 SUNNY answers (Q5 after upload), merge #54/#58 (HOLD lifts after 18b + chat's last look), #65 UI, record, submit 29 Oct.
- Backlog: decision 072 should note `action-manifest.test.ts` at the release commit still expects the old `version:` line (fixed on main).

## 28 Oct 19:25: seed-01 scored: 9/22 caught, 0/8 controls flagged; numbers.md §7 filled; codex-18 fills the drafts
- Report: [seed-01a out](prompts/2026-10-27/seed-01a-agent-runs-it.out.md). 30/30 final runs green (Recall `0aeef06`, Linea `86fd5d7`). Automatic rule: in-diff 5/6, cross-file typed 2/6, keyed 2/6, semantic 0/4; all 9/22 (Recall 6/11, Linea 3/11); controls 0/8 commented. **3 of the 4 cross-file catches are the deterministic contract check**, 1 the model. 17/30 PRs got no review.
- Chat rulings: headline = automatic rule only; manual column unused (Sunny is an interested party; the 2 near-misses are described, not counted). Video: model catch recall#4 first; contract catches (recall#7, #6) narrated as "deterministic, no model".
- numbers.md §7 filled (allowed sentence, table, deviations, links). → [codex-18](prompts/2026-10-28/codex-18-fill-seeded.md). Needs `tracehound-seeds` public first.
- UI: ui-11's `recall-seeded` mapping flag can be turned on now (scoring done).

## 28 Oct 17:50: seed stop 3: linea#1's re-trigger ran the old pin (stale test-merge commit); ruling: open the 14 now, retry #1 after
- Run 40589574980 on linea-seeded#1 downloaded `0aeef06`: the PR's merge_commit_sha `affb271` still had parent `3012a54` (old main); GitHub hadn't rebuilt it after main moved to `6d8771a`. Chat's ruling-2 premise ("ready_for_review uses the base branch's current workflow") was wrong. See lessons.
- **Ruling (chat):** (1) open the 14 remaining Linea PRs now: they merge against `6d8771a` and run 2.1, which is the real end-to-end test; STOP if the first one fails at install. (2) Meanwhile poll `pulls/1` until `merge_commit_sha`'s parents include `6d8771a`, then toggle draft/ready once more (recorded). (3) If it hasn't rebuilt when the 14 are done, one base re-set to `main` (no content change) is allowed as a disclosed deviation. (4) Then score all 30.

## 28 Oct 17:15: codex-17 done (pins on 2.1); seeded Linea half not opened yet
- codex-17 ([out](prompts/2026-10-28/codex-17-pin-2.1.out.md)): #54 `a69d0d0`, #58 `1ea041b`, demo repo `c3ae128`. Chat checked: Try-it pins `86fd5d7`; remaining `0aeef06` mentions all describe the measured pipeline; no competitor names in writeup/video-script (Devin is listed only as a coding assistant: fine, not a benchmarked reviewer). Tests 1,441 + 8 skipped.
- GitHub (17:10): linea-seeded workflow pinned to 2.1 (`6d8771a`) but **only 1 PR exists on linea-seeded** (recall-seeded: 15). Seed-01a session state unknown → asked Sunny. Scoring, numbers.md §7 and every TBD-SEEDED wait on it.
- Schedule slip: the video was planned for this afternoon; its shots 2–3 need the seeded result.

## 28 Oct 03:20: review-freeze-2.1 ready (#68 OK to merge, tag 86fd5d7); Recall's 15 seeded runs done
- **Release** `86fd5d796c901cb20177cda28c4925e8c72d0901` on `release/review-freeze-2.1`: chat checked `git diff 0aeef06 86fd5d7` = `action.yml` only (pnpm/action-setup removed; setup-node 24 then `npm install -g pnpm@11.20.0` in the action checkout; later steps unchanged). Untagged until Sunny tags.
- **#68** (head `f9c669a`, CI 10/10): diff vs main 6 files: action.yml, install-matrix workflow, dogfood workflow pin → 2.1, decision 072, freeze.md, manifest test regex. Matrix: old action.yml fails pnpm@9 **and pnpm@10**; fixed passes all 6. **OK to merge (commit).** Notes for the final pass: (1) at the release commit `action-manifest.test.ts` still expects the old `version:` line (fixed on main only; a test, not behaviour) — add to 072; (2) the dogfood review workflow made paid calls on #68 twice (~cents; it's the dogfood lane, expected).
- Not proven end to end with a version mismatch: linea-seeded#1's re-trigger on 2.1 is that test.
- numbers.md §0.5/§7 updated for 2.1. → [codex-17](prompts/2026-10-28/codex-17-pin-2.1.md): pins in #54, #58, demo repo; Devpost "challenges" gets the seeded-test install bug.
- **Recall seeded (15/15 green, first attempt, no other bots):** review with 1 inline finding: #3, #4, #9, #10, #12; header-only review (0 findings): #2, #6, #7; no post: #1, #5, #8, #11, #13, #14, #15. All headers 3.5. Not scored yet (after Linea).

## 28 Oct 01:45: seeded test found an Action install bug (pnpm `packageManager`); Devin Review auto-posting on mirrors
- Gate passed: `recall-seeded#2` (s25) posted a review, header `pool-verify-v1 · nvidia/Nemotron-3.5-Super-VL`, 0 findings (pool 1, kept 0), 182.7 s. `recall-seeded#1` (s15) diff 2 files +4/−3 (not zero-chunk) → result: no post.
- **Bug (real, judge-facing):** `linea-seeded#1` (s29) red twice: composite action's `pnpm/action-setup@v4` with `version: 11.20.0` fails "Multiple versions of pnpm specified" because Linea's root `package.json` has `packageManager: pnpm@9.0.0`. Any user repo pinning another pnpm can't run the Action. Before any model call.
- **Fix → review-freeze-2.1** ([devin-03](prompts/2026-10-28/devin-03-action-install-fix.md)): branch from `0aeef06`, one-file diff (`action.yml` install step only, review code byte-identical); proof on main with an install matrix (pnpm 9/10/11, yarn, npm, none; no key, review step skipped); decision 072. Sunny tags after chat check. Then re-pin: linea-seeded workflow, README Try-it (#54), demo repo; numbers.md §0.5 gets "Action at 2.1, pipeline = review-freeze-2".
- **Devin Review** (account-wide app) auto-reviewed the 3 seeded PRs (model calls by a third party; comments not scored). Sunny removes the app's access to both mirrors before more PRs open. Deviation disclosed.
- **Plan** ([ruling 2](prompts/2026-10-28/seed-01a-ruling-2.md)): Recall's remaining 13 tonight on `0aeef06`; Linea's 15 after the 2.1 pin (linea#1 re-triggered via draft/ready, not closed). Results per mirror with their Action commit.

## 27 Oct 23:45: seed-01a stopped at its first-PR gate; ruling: continue with a 3-PR gate
- Agent: step 0 all pass (hashes, scorer 13/13, ADMIN, secret on both, 15/15 seed branches each). Workflow committed: linea-seeded `3012a54`, recall-seeded `296f8f3`. Script copy `runs/open-prs-seed01a.sh` (`dbd3bdd`): no CodeRabbit comment, 90 s, cd to repo root (disclosed). `recall-seeded#1` (s15): run 40556700220 green, model 3.5 at `0aeef06` in the log, reviewer step 13.5 s, "reviewer returned no findings; nothing posted".
- **Chat (read `0aeef06`):** a wrong key/route/model makes every chunk fail → `StageError("reviewers")` → "reviewers failed…" (`pipeline.ts:134`). "returned no findings" (`action.ts:293`) means valid model replies with zero findings, unless the diff made zero chunks. So s15 = result (miss, no post). → [ruling](prompts/2026-10-27/seed-01a-gate-ruling.md): record the diff size, open 2 more; continue if any posts a 3.5 review; STOP if all 3 are no-post under 30 s.
- The run was 18:05 UTC = 23:35 IST, after Sunny's $38.78 reading (23:31), so the $4.74 drop is not seed-01a.
- Watch: demo PR #2 also posted nothing under 3.5 (24 Oct). If seeded PRs show the same pattern widely, that is the honest result (3.5's precision comes from saying less).

## 27 Oct 23:35: Token Factory balance $38.78
- Sunny (23:31): **$38.78** remaining. Previous reading $43.52 (24 Oct 10:45) → **−$4.74 gross (≈ $4.02 pre-VAT)** since. Expected in that window: 071 judges $0.19, dogfood/CI cents, plus billing lag from 24 Oct (the 24 Oct lesson: the balance trails billing by hours, so late #333 / 070 charges may land here). Not yet explained call by call; check the console's usage by day before reading anything into it.
- seed-01a (~$3 with VAT) is next; expect ≈ $35–36 after it. If it ends lower than ~$34, stop and look.
- `nebiusCapUsd` stays 0 (it governs relay only; seed-01a spends in GitHub Actions on the mirrors).
- numbers.md §4's "$43.52 balance" stays: it is the balance the no-final decision (P008 amendment 8) was made against.

## 27 Oct 22:50: #67 merged (main `7bc1731`); ui-11b in the UI session; seed-01 handed to an agent (seed-01a)
- Sunny merged **#67** (Bugbot never replied; defaults proven identical). ui-11b pasted into the UI session; #65 merges when Sunny's UI changes settle.
- **seed-01a** ([brief](prompts/2026-10-27/seed-01a-agent-runs-it.md)): Claude Code account A, plain session in `~/Projects/tracehound-seeds`. Sunny only sets `NEBIUS_API_KEY` on both mirrors from `.env` via `gh secret set` (key never passes through an agent). Agent: verify hashes + scorer checks → workflow on both mirrors (pinned `0aeef06`, 3.5) → one PR first, check header model → rest at 90 s → wait/rerun failed once → score → RESULTS.md → inbox (counts + links only). **Bugbot skipped** (disclosed). Mirrors public, 16 branches each; `tracehound-seeds` private.

## 27 Oct 22:40: codex-16 done (#54 `d762b14`); live /benchmark is v2; **HOLD #54 and #58**
- **HOLD #54, #58:** #54 is non-draft and still has 2 `[[TBD-SEEDED]]`; #58 has TBD-SEEDED + SUNNY slots. Merge only after seed-01 fills them and chat's final check.
- codex-16 ([out](prompts/2026-10-27/codex-16-readme-first-screen.out.md)): `652bf8e` merge of main (README = branch side), `d762b14` first screen: "See it without a key" (dashboard, demo, numbers.md §1 sentence, local no-key route), key/fork/cost/partial-review note moved before the YAML, `pnpm demo` heuristic-names + `git checkout -- snapshots/`. Chat: PR diff vs main is README only; wording matches numbers.md §1/§8; links 0 broken (Codex).
- Live `https://tracehound-tau.vercel.app/benchmark` (WebFetch): links `b0f1bc4`, shows v2 (57.1%), no undefined/NaN. Auto-deploys from main.

## 27 Oct 22:05: #66 merged (main `e278e04`); devin-02b checked; codex-16 stopped on a README conflict → codex-16b
- **#67** now `b4b65b5` (CI 5/5): `e9c9a6b` cacheTruncated false also misses on read; `b4b65b5` lenient parser takes the last candidate passing the strict check, else strict. Chat: diff 5 files, nothing under `packages/action/test/` changed (identity snapshot untouched), defaults unchanged in code; local post-freeze-options 45/45, action 249/249. **OK to merge (commit)**; Bugbot still silent — if no reply by 28 Oct 10:00, merge anyway (defaults proven identical; Devin Review + chat reviewed).
- **codex-16** stopped at step 0 (correctly): `git merge origin/main` conflicts in README.md ~l. 245 (the branch's 3-line "first look, v1" intro vs main's #66 edit nearby). Chat reproduced it: the hand resolution equals the branch side byte for byte. → **codex-16b** ([brief](prompts/2026-10-27/codex-16b-resolve-and-continue.md)): abort, re-merge, `checkout --ours README.md`, then the edits.

## 27 Oct 21:30: devin-02 report; devin-02b written
- [devin-02 out](prompts/2026-10-27/devin-02-flagged-fixes-and-research.out.md): #67 CI 5/5 green, `bugbot run` posted (no reply yet). Devin Review found 2 real issues, **both only under non-default options**: `cacheTruncated: false` still serves old cut-off entries on read (`client.ts:253-265`); the lenient parser takes the first fence containing the key, even `{"findings":null}` (`review.ts:111`). → **devin-02b** ([brief](prompts/2026-10-27/devin-02b-two-option-fixes.md)): fix both, identity snapshot unchanged. #67 merge waits for 02b + CI + Bugbot.
- Research (docs only): Token Factory documents `json_schema`/`json_object` in general, nothing confirming 3.5 VL on us-north1; only `enable_thinking` is documented in `chat_template_kwargs`, no budget on Token Factory (matches our 23 Oct probe); NVIDIA recommends temperature 1.0 / top_p 0.95 for 3.5 text (0.6 for vision); reasoning + JSON together undocumented. Smoke plans in the #67 body, not run.

## 27 Oct 21:20: fill-01 done (#66 OK to merge); devin-02 = #67 OK to merge after CI + Bugbot; codex-15 in; ui-11's benchmark page breaks on v2 tables
- **#66** (`docs/fill-results`, head `6c45378`, CI green): chat checked. 6 files, all in scope. results.md v2 (9 tables; table 9 = dev cost); numbers match sources and numbers.md (spot-checked: Cal.com run 4 cost, 13:38:03Z/13:38:51Z, 24 gaps, levers-v2 cap cells incl. the 4 pool-raw cells, vote −4.2/+6.3, verifier +10.9/+6.7). benchmark.ts notes follow §0 ("all 50 PRs" only in titles 3–4, no "3rd of 6"). **OK to merge (commit).** Then check /benchmark on Vercel shows v2.
- **#54** head `2bb3144`, **#58** head `e596309`: TBD-ULTRA/LEVERS filled, claims fixes in (verified by grep: no names, no 1,305, no "26 dated", no "fails the build", no "doesn't pay"). Still open: TBD-SEEDED, 5 SUNNY. Not ready.
- **#67** (devin-02, head `12d7f3c`): chat checked. Identity snapshot committed first (`1a7302f`); changed only at `54460b3` and only by additive `attemptsDetail` in stage outputs, never in `posts`. Defaults verified in code (`parser` strict, `retryOnLength` append, `cacheTruncated` true, `capOrder` pool, `poolNullLines` separate). Local Node 22: action 249/249, analyzer 1001 + 7 skipped, viewer 188/188, typecheck green. Devin's own note: `poolNullLines: merge` is a no-op for today's reviewers (null-line findings are dropped as malformed before the pool). **OK to merge after CI green + Bugbot.**
- **codex-15** ([out](prompts/2026-10-27/codex-15-judge-path-smoke.out.md)): install/test/typecheck/dev server/pages/Action ref/inputs/YAML/missing-key guard pass. Fresh clone: 1,394 passed + 7 skipped (docker/env-gated files skipped; fill's 1,421 is Sunny's machine). **CI and repo are Node 24** (the handoff's "Node 22" note was wrong; chat's local checks on 22 still pass). Real findings: README first screen needs a key before showing anything and has no dashboard URL; `pnpm demo` without a key silently uses heuristic names and overwrites snapshots. Viewer build failure = sandbox (Google Fonts, port), not the repo. Live /benchmark is v1 until #66 deploys. → **codex-16** ([brief](prompts/2026-10-27/codex-16-readme-first-screen.md)).
- **ui-11 (#65) problem:** its `benchmark/page.tsx` derives tiles by table position and old-note regexes; on #66's tables it would show wrong/undefined values, and its caption "no gap … outside its 95% interval" is false for v2 (CodeRabbit). Textual merge is clean, so CI wouldn't catch it. → **ui-11b** ([brief](prompts/2026-10-27/ui-11b-benchmark-page-v2.md)), after #66 merges.
- Merge order: **#66 → ui-11b on #65 → codex-16 on #54 → #67 any time.**

## 27 Oct 20:30: devin-02 written (flagged fixes + docs research, same Devin session)
- [brief](prompts/2026-10-27/devin-02-flagged-fixes-and-research.md). One PR `devin/post-freeze-fixes`: identity test first (default output byte-identical), observability (default on, additive), then truncation-aware retry, lenient parser, line-less pool merge, agreement-first cap, no caching of cut-off replies (all **opt-in, default off**), a $0 cache-scan script, `docs/post-freeze-fixes.md` ("not measured"). Research: Token Factory JSON schema + reasoning budget for 3.5, NVIDIA sampling for 3.5.
- Files disjoint from fill-01 and ui-11. No Action inputs, no workflow or prompt changes; demo/seeded/README stay pinned to `0aeef06`.
- **Merge rule:** before submission only if CI + identity test green, chat checked, Bugbot run; else after 30 Oct. Ready by 28 Oct 09:00.

## 27 Oct 20:00: devin-01 report in, checked by chat; nothing changes before submission
- Report: [devin-01 out](prompts/2026-10-27/devin-01-codebase-review.out.md). Relay (fill-01) and Codex (codex-15) running.
- **Verified against main `5ba60a0`:** (1) the reviewer retry ignores `finishReason` and retries a 32k-cut reply with the same `max_tokens` and reasoning on (`review.ts:178-196`; same loop in `review-ctx.ts`): real; (2) the cache keeps any non-empty reply, so a cut-off reply with partial content replays on cached reruns (`client.ts:296`); **empty-content truncations are not cached**, so the dev failures that were pure reasoning have no body on disk; (3) the fence regex is anchored `^…$` (`review.ts:83`): real; (5) `agrees()` needs both lines non-null, so line-less duplicates both enter the pool (`agree-rule.ts:7-8`): real; cap order is pool order (`action.ts:163`, `findings.slice`): real; VERIFY_FAILED findings are kept (`verify-ctx.ts:90`): real, by design.
- **Overclaim in the report:** "levers-v2 showed the 3.5 raw pool beats verified output". It didn't: on 3.5 both differences (−1.6, −5.4) have intervals including 0 (numbers.md §9: open question). The two-sample verifier idea stands as an idea, not as a fix for a measured loss.
- **Not done before 30 Oct** (product frozen at `review-freeze-2`; a fix is a new unmeasured config). Post-hackathon order: (a) $0 cache scan of 3.5 dev bodies: `finish_reason=length` with empty vs partial content (turns "not checked call by call" into a count); (b) finishReason-aware retry + lenient parser + don't cache unparseable replies, one dev run; (c) `response_format` / reasoning-budget smoke on 3.5; (d) T 1.0 self-consistency (≈3× reviewer cost).
- **For chat's post-fill pass:** add one "known limit" line to README/Devpost (found in review: the retry doesn't distinguish truncation from malformed JSON) and to numbers.md §9. Backlog fix: the merge-base fetch duplicate is not in `viewer/lib/data.ts` (Devin: no such file).

## 27 Oct 18:45: #63 and #64 merged (main `5ba60a0`); fill-01 and codex-15 written; claims check of #54/#58 done (pre-fill)
- GitHub: main `5ba60a0` = merge #64 on merge #63. Relay unblocked. ui-11 started by Sunny on account B (`~/Projects/TraceHound-ui`, `ui/v2`).
- **fill-01** ([brief](prompts/2026-10-27/fill-01-results-and-drafts.md), relay A, $0): results.md + `viewer/lib/benchmark.ts` on v2 (8 tables; "all 50 PRs" only in tables 3–4 titles; no "3rd of 6" in the viewer); ReviewBench test runs into `reviewbench.md` (they were only in the brain); v3-recall wording fixes + Bugbot known defects; then #54/#58 TBD-ULTRA/LEVERS and the claims fixes below. Disjoint from ui-11's files; STOP if `benchmark.test.tsx`/`page.tsx` need edits.
- **Claims check (chat, on #54 `64082a7` / #58 `8184e54`)**, fixes go in fill-01: "verifier doesn't pay on 3.5" is an overclaim → §9 "open question"; "1,305 tests on 23 Oct" not found in the repo → fill's own `pnpm test` count (levers-v2 had 1,371 on 27 Oct); "26 FEEDBACK entries" is 29 at `5ba60a0`; "79 of 624" ✓ (`dev-3.5.md`); "$0.0113 ledger gap" ✓ (results.md §8); "a test fails the build" → CI; drop Cursor from the assistants list. Verified ✓: 067 x1 interval includes 0, 35/35 404s, AISTUDIOSUP-1966, 050's 11 vs 13 / 28 vs 26 / 0 of 80, cc-01's five overclaims, 43 retry-valid chunks, pipeline constants (60k / 40k / 24k chars, us-north1 route). After the fill, chat checks only the new text.
- **codex-15** ([brief](prompts/2026-10-27/codex-15-judge-path-smoke.md), Codex, $0, report only): fresh clone, follow #54's README as a judge (install/test/viewer/demo-without-key/toy harness/Action block/links/live dashboard).
- Critical path is **seed-01 part A** (Sunny): the video's shots 2–3 and every `[[TBD-SEEDED]]` wait on it.

## 27 Oct 18:00: handoff Oct27; #63 and #64 checked (OK to merge); bunny-06 done
- **#63** (071b, `b5a5f2e`): chat checked (9 files, no product paths, llm tests 55/55). Ultra 2-run pool-verify v1 F 47.1 vs 3.5 50.4 (no interval-backed difference); pool-raw −10.2 [−17.7, −3.2] vs 3.5. OK to merge.
- **#64** (codex-14 levers v2, `a149e17`): $0 replay, 4 new files; no verified-pipeline cap gain; verifier on 3.5 inconclusive; 2-of-3 vote no gain; 35/74 Medium+ missed by both 3.5 runs. OK to merge after CI. numbers.md §5 and §9 filled.
- **bunny-06** done: #54 `64082a7`, #58 `8184e54`. Open: TBD-SEEDED/ULTRA/LEVERS, 5 SUNNY questions; chat claims check pending; drop "Cursor" from Devpost.
- Handoff: [Oct27](history/TraceHound_Handoff_Oct27.md).

## 27 Oct 17:45: lanes rebalanced (all UI on Claude B; Gemini research only)
- Sunny: all UI on **account B** (`claudeb`), inspired by the Vite / Vite+ sites, with his own hero design pasted into the session. Gemini hallucinates on build tasks → research only.
- **ui-11** ([brief](prompts/2026-10-27/ui-11-ui-v2-claude-b.md)) replaces ui-10 and gemini-02: expand fix → PR lane → pinned findings → redesign (tokens, graph, landing from Sunny's hero, reviews, benchmark) → Recall snapshot. Worktree `~/Projects/TraceHound-ui`, branch `ui/v2`. Due 28 Oct 12:00.
- **Account A** now carries: 071b → the fill (results.md + viewer benchmark data, after ui-11 merges or coordinated) → seed-01 scoring.
- gemini-01 dropped: chat does the claims check on bunny-06's PRs.
- Not doing: a new demo project with planted bugs (staged-demo credibility; the pre-registered seeded mirrors are the honest version).
- Orphan warnings were not stale: Recall `20ac149` really had 2 unimported files; Sunny deleted `SidebarToggle.tsx` → Recall `62eaca1`.

## 27 Oct 02:45: all-hands plan; lane board
Sunny: "do everything now", with 6 lanes. Chat's line: the product stays at `review-freeze-2`; improvements ship as report-only levers v2 + "what's next". Single source of truth: [submission/numbers.md](submission/numbers.md).
| Lane | Job | Brief | State |
|---|---|---|---|
| Relay (A) | 071b: run 11 + Devin's 4 fixes on #63 → ready | [071b](prompts/2026-10-27/071b-run11-and-devin-fixes.md) | to start (needs `chain done`) |
| Relay (A) | the fill: results.md + viewer /benchmark (after 071b) | not written yet | — |
| Codex | levers v2 offline replay ($0) | [codex-14](prompts/2026-10-27/codex-14-levers-v2-replay.md) | to start |
| Bunny (B) | README #54 + Devpost #58 + video script | [bunny-06](prompts/2026-10-27/bunny-06-readme-devpost-video.md) | to start |
| Sunny + Bunny | seeded PRs on the current config, then scoring | [seed-01](prompts/2026-10-27/seed-01-sunny-and-scoring.md) | Sunny's hand steps A1–A5 |
| Gemini | claims verifier (after bunny-06) | [gemini-01](prompts/2026-10-27/gemini-01-claims-verifier.md) | waits |
| Cursor | UI (viewer/, not /benchmark tables) | waits for the UI chat's state | — |
| Devin | reviewer on every PR | — | on (found 4 real bugs on #63) |
- Devin's #63 findings, checked by chat: baseline mismatch doesn't stop (report script); lowercase Ultra id priced $0 on any route (latent cap bypass); verifier truncations missing from counts; cache key ignores route (latent). None changes run 10's numbers. Fixed in 071b.

## 27 Oct 01:50: back from Orchestrate; replan for the last 3 days
- GitHub (01:50): main `36de717` (#62 merged). Open: #54 README (draft), #58 Devpost (draft), #63 071 (draft, head `6bd72e1`, run 10 only). No new `ui/*` branch since 23 Oct. Inbox empty. Run 11 status is on Sunny's Mac (`runs/071-chain.log`).
- **Not done on 26 Oct:** the seeded test, the fill, the UI redesign merge (was due 27 Oct noon).
- **Chat's proposed plan** (Sunny to confirm):
  - 27 Oct: 071b (fold run 11, $0, short) → **the fill** (results.md + viewer + README #54 + Devpost #58: 069, 069b, ReviewBench test runs, 070 negative, 071 Ultra; demo wording) → UI merge if ready.
  - 28 Oct: video script + recording; Devpost text final; Grok verifier on posts.
  - 29 Oct: submit. 30 Oct: buffer only.
  - **Seeded test: cut** (chat's recommendation). No brief exists, tools aren't installed on the mirrors, Greptile consent was never sent, and R14 keeps competitor names out of the video and Devpost anyway. README says it wasn't run.
  - **Demo risk:** under 3.5 at freeze-2, demo PR #2 posts no findings (24 Oct). Judges need one live PR with a real TraceHound review on the current config. Decide in the fill.

## 24 Oct 16:05: 071 run 10 in (PR #63 draft); run 11 running detached; relay stopped for Orchestrate
- Report: [071 out](prompts/2026-10-24/071-dev-ultra-nvidia-api.out.md). Draft **PR #63** (head `6bd72e1`, CI green), run 10 only. Plan `cb58934` before the probe. Relay reviewer: CONTINUE, numbers recompute.
- **Run 10 (Ultra, v1):** pool-verify-v1 P 34.8 / R 54.1 / F 42.3 (3 Super mean 42.3, 3.5 mean 50.4); pool-raw P 31.2 / R 64.9 / F 42.1 (3 Super 38.2, 3.5 53.9) — **worse than 3.5 on pool-raw, interval excludes 0**; elsewhere no interval-backed difference. x1: 54.6 / 51.8. Judge spend $0.10. 3 request timeouts at 300 s (rerun).
- **Run 11** started detached at 09:52Z (`runs/071-chain.sh`, nohup), 4/40 at last check; it scores itself and writes `runs/071-report-final.txt` + `chain done`.
- Relay's auto-continued follow-up (038: fold run 11 into #63) is **stopped by Sunny** to save Claude tokens for Orchestrate. On 26 Oct chat turns it into brief **071b** (same steps). Check the tree for an untracked `docs/prompts/071-followup-run11.md` first.
- Small fixes for 071b (from relay's reviewer): "most reliable model" → counts only; commit ref `0dec002`, not `e997359`'s follow-up.

## 24 Oct 11:35: no top-up → no ReviewBench final (P008 amendment 8); Ultra goes to dev via relay 071
- **Sunny (11:30): no top-up; work with the $43.52.** So no final and no leaderboard row. ReviewBench result = the six 25-PR test runs, labelled as such ([P008 amendment 8](decisions/P008-reviewbench-preregistration.md)). Best: candidate 3 grounded F1 21.3 / P 76.3 / R 12.4.
- **Ultra on ReviewBench: impossible** (egress locked at registration to Token Factory us-central1/us-north1; Ultra isn't free there). Amendment 6 dropped.
- **Relay 071 (written):** [071-dev-ultra-nvidia-api](prompts/2026-10-24/071-dev-ultra-nvidia-api.md). pool-verify-v1 (+ derived pool-raw) on Ultra via `integrate.api.nvidia.com`, dev runs 10–11, opt-in provider (`TRACEHOUND_NVIDIA_API=1`), 36 RPM throttle, 429 backoff, $0 price, Nebius cap $3 (judges). Report only. Runs unattended during Orchestrate.
- **Remaining Nebius uses** of the $43.52: 071 judges (≤ $3), the 26 Oct seeded test (TraceHound on 3.5, about 30 PRs, a few $), dogfood. Comfortable.

## 24 Oct 11:25: #331 measured: the x2 final costs ~$107 with VAT; it doesn't fit $43.52
- Source: #331 agent output (Sunny's zip; our own logs only, no golden): **1 round, 25 PRs, all ok** (1 partial 1/4), `cost_usd` sum **$3.45** pre-VAT (≈ $4.07 with VAT, consistent with the balance drop).
- Per PR: mean **$0.138**, median $0.073, max $0.983 (one PR, 1441 s). Heavy tail; bootstrap 90% for the mean $0.083–0.208.
- **Final = 219 × 3 = 657 PR runs → ~$90 pre-VAT, ~$107 with VAT (90% range ~$65–160).** Balance $43.52.
- Rough others (scaled, not measured): candidate 2 pool-raw ≈ half → ~$53 with VAT; still over the balance.
- Options for Sunny: (A) top up and run candidate 3 as selected; (C) candidate 2 with a money-driven departure disclosed (still needs a top-up); (D) no final: publish the 25-PR test-run numbers only, labelled. Chat's recommendation in chat.

## 24 Oct 10:45: console is $43.52, not ~$60. HOLD the final until #331's cost is measured
- Console (usage updated 03:45 UTC): **balance $43.52**; 30-day Token Factory subtotal $57.05 + **VAT $10.27 (18%)** = $67.31.
- **Why the plan was wrong (chat):**
  1. **$68.09 (02:26) was stale.** From $70.69 (23 Oct 17:06) it implied only $2.60 spent across five test runs. The balance lags billing by hours.
  2. **VAT.** Every chat estimate and the local ledger are pre-VAT; the balance pays ×1.18.
  3. **Test runs cost ~2–3× the estimate.** 70.69 − 43.52 = $27.17 gross ≈ $23.0 pre-VAT, minus 070 $5.71, 069 ~$4 and smokes/CI → about $2–2.5 per 25-PR test run, against ~$1.3 assumed. Cross-check: month subtotal $57.05 − ledger $40.24 = $16.8 off-ledger (6 test runs + smokes + dogfood).
- **Implication:** if a test run is 25 PR executions, the final (657) is ≈ 26 × test-run cost → candidate 3 (x2, the costliest 3-Super config) may need **$70–100 with VAT**. Doesn't fit $43.52.
- **Action:** Sunny reads #331's agent output (our own log: per-PR spend lines, not golden) → chat computes the final's cost × 1.18 + 15% margin → top-up amount. Selection stays candidate 3; switching to a cheaper config for money would be a departure, and it's Sunny's call only if he won't top up.

## 24 Oct 10:35: Bugbot on #62 (dry run): 2 real bugs, both in recall-v3, neither changes 070's numbers
- Bugbot (default effort, manual `bugbot run`, commit `cf55841`) posted 2 Medium findings in `packages/analyzer/src/review/recall/run.ts`. Chat checked both against the code: **both are real.**
  1. **Guard drops completed lens findings** (l. ~299): when the wall-clock `Promise.race` fires during lens calls, `comments` is still `[]`, so findings already in `raw` are never reduced and the review is empty. Breaks the "findings so far" rule. **Dev impact: none**: guard hit 0 in both arms.
  2. **Lens findings lose chunk order** (l. 257–260): `raw` is filled in completion order and sorted by producer only, so dedup survivors and the 60-cap depend on timing. Reduce is documented as chunk-ordered. **Dev impact: small** (cap hit on 1 S PR; dedup picks which duplicate's text survives); one sampled run at T 1.0 is noisy anyway. Verdict unchanged.
- relay's reviewer and the chat's check both missed these. **Not fixed:** v3 never runs again. The 26 Oct fill adds a "known defects (found by Bugbot)" note to `v3-recall.md`. Merge #62 as is.
- Seeded test: Bugbot works on relay PRs with manual trigger; good sign for 26 Oct.

## 24 Oct 10:20: #333 candidate 6 = 20.4, so the P008 winner is candidate 3; final = pool-raw-x2 · 3 Super
| # | candidate | G F1 | G P | G R | A F1 | A P | A R | findings (novel) | avg time |
|---|---|---|---|---|---|---|---|---|---|
| 333 | 6 pool-raw-missed · 3 Super | 20.4 | 64.5 | 12.1 | 28.9 | 43.9 | 21.5 | 194 (63) | 4m20s |
- Candidate 6 needed ≥ 24.3; it scored 20.4, below candidate 3 (21.3) too. The missed pass added findings over candidate 2 (124 → 194) but lost precision.
- **Selection (P008 + amendment 7, applied by chat):** v3 is closed (070), candidate 6 fails its margin, and candidate 3 leads 1–5 by 5.3 (no tie). **The one final run = candidate 3: `pipeline=pool-raw-x2`, `model=nvidia/nemotron-3-super-120b-a12b`, image `9b9a5127`.**
- Money: console estimate about $60 (68.09 − #333 − 070's $5.71). The x2 final is unmeasured, estimated at about $40. Sunny reads the console before starting it: ≥ $50 → start; under that → top up first.

## 24 Oct 10:30: 070 done, v3 fails P009; PR #62 checked (no HOLD); the final goes to the P008 winner
- **Relay 070 → draft PR #62** (head `cf55841`, CI green). Relay stopped for Sunny (STOP_FOR_SUNNY, 4 questions). Report: [070 out](prompts/2026-10-23/070-v3-recall-mode.out.md), [relay review](prompts/2026-10-23/070-v3-recall-mode.relay-review.out.md).
- **Result (dev, v1):** S (3 Super) P 14.0 / R 50.0 / F 21.8 vs floor 20.0; V (3.5) P 20.9 / R 63.5 / F 31.4 vs floor 38.0. **Neither qualifies → no v3 image, smoke or test run.** Lens B and the missed pass buy recall with precision; even V's lens A alone (P 25.7) is under V's floor.
- **Chat's check of #62:** recomputed both arms and both floors (29.98 → 20.0; 47.95 → 38.0); `.github` change is one options line; `entry.ts`/`adapter.ts` only add recall-v3 branches; P009 rule sections copied verbatim (only the header/disclosure lines differ); `recall-v3.test.ts` 20/20 and `scripts/reviewbench` 87/87 on Node 22; `tsc -p scripts/reviewbench` clean. **No HOLD.**
- **Rulings (chat):**
  - Stop rule: **recorded as a departure**, data accepted. The brief's wording was ambiguous (chat's fault, see lessons); arm V was pre-registered and ran once, so its number is valid. Cost $3.33.
  - Refusal counts: different units (per call vs per chunk/PR), not a data error. Fix the wording and `v3-recall.md:11` in the 26 Oct fill, not a relay cycle now.
  - **The v3 measurement was hampered** (24% / 36% lens-call failures, mostly truncation → invalid JSON; P009's own $0.12 per-PR cap starved multi-chunk PRs). Results must say so. No rerun: precision fails even on the clean lens-A subset, and there is no time or money.
- **Measured cost per PR** (v3 S): $0.055 → R16's ~$33 final estimate was low. The P008 final (x2) is still unmeasured; read it from the console delta of #331.
- **Next:** #333 (candidate 6) result decides the final's config: ≥ 24.3 → candidate 6, else candidate 3 (pool-raw-x2 · 3 Super). Sunny starts the final before 18:00.

## 24 Oct 03:30: 070 branch progressing; Bugbot on (manual)
- `build/070-v3-recall` at `8ffc2a6` (02:43): pipeline, CLI, ReviewBench pipeline, tests; **lens prompt shas committed at 02:39, before any v3 call** (P009 rule OK so far). Reasoning-budget probe: neither model honoured the budget, so both arms use the fallback. No PR yet; dev arms running.
- Inbox empty. Test run #333 (cand 6) result still to come from Sunny.
- **Bugbot set up (03:47).** Via the X Subscription (Cursor Premium); config at cursor.com/automations/from-cursor/bugbot. 1/54 repos enabled (TraceHound only), Trigger **Manual Only**, effort Smart. Chat asked Sunny to set **Autofix Off** and **PR Summaries Off** (neither may touch a measured branch or a relay PR body). Use: comment `bugbot run` on the 070 PR. Billed from the shared X quota (14% used, resets 29 Oct; credits $0).

## 24 Oct 03:05: Sunny has build.nvidia.com free API access (40 RPM), including Nemotron 3 Ultra
- **Idea:** run Ultra for free there.
- **Chat's call:** yes, but as a **dev-set, report-only** row ("does the biggest Nemotron help?"), at $0.
- **Not on ReviewBench:** its egress is locked to the two Token Factory hosts, and the hackathon's runtime requirement is Token Factory. So the product and ReviewBench stay on Token Factory, and the NVIDIA-API runs are disclosed as a side experiment.
- **Needs a small client change**, done **after 070 merges** (relay owns `packages/analyzer` tonight):
  - a per-model endpoint + key for a non-Token-Factory host (`integrate.api.nvidia.com/v1`, key `NVIDIA_API_KEY` in `.env`);
  - a $0 price entry so the ledger doesn't book fake spend at the $5/$15 placeholder fallback;
  - 429 backoff / ≤ 40 RPM throttle.
- **Arms (proposal):** recall-v3 (whichever P009 picks) and pool-verify-v1 on Ultra over the 40 dev PRs, v1 + x1 judges (judges stay on Token Factory). Report only. If Ultra is clearly better on dev, that informs amendment 6 (a paid Token Factory Ultra test run) if money is left after the final.
- **Confirmed (03:12):** `https://integrate.api.nvidia.com/v1`, model id **`nvidia/nemotron-3-ultra-550b-a55b`** (lowercase), HTTP 200. It returns `reasoning_content` (reasoning on by default; 5 tokens → `finish_reason: length`), so the brief must budget reasoning tokens. The key is in Sunny's shell / `.env` as `NVIDIA_API_KEY`.
- **Rate limit: 40 requests/minute** (HTTP 429 above it). A 40-PR recall-v3 dev run is about 300–500 calls, roughly 8–13 min plus latency. The client needs a throttle (≤ 40/min, wait on 429 with backoff), not full parallelism.

## 24 Oct 02:55: candidate 6 smoke green; test run #333 queued
- Smoke `pool-raw-missed` · 3 Super on `9b9a5127` (02:48, green): idx 0 ok, 3 findings (pool 2, missed 1, new 1), $0.0432, 158 s; idx 4 ok, 1 finding (pool 1, missed 0, new 0), $0.0076, 37 s.
- Config added (`pipeline=pool-raw-missed`, `model=nvidia/nemotron-3-super-120b-a12b`). **Test run #333** started 02:54:19 (queued). It must reach ≥ 24.3 to beat candidate 3.

## 24 Oct 02:35: handoff Oct24; relay restarted on 070
- Relay hit its session limit during 070 and was restarted about 02:30 (Sunny). Candidate 6's smoke on `9b9a5127` is next (Sunny).
- main `f488cef` (#61 merge). Handoff: [Oct24](history/TraceHound_Handoff_Oct24.md).

## 24 Oct 02:30: test runs 5/6 done. Candidate 3 (pool-raw-x2) leads at 21.3; v3's bar is 24.3; money is tight
| # | candidate | G F1 | G P | G R | A F1 | A P | A R | findings (novel) | avg time |
|---|---|---|---|---|---|---|---|---|---|
| 331 | **3 pool-raw-x2 · 3 Super** | **21.3** | 76.3 | 12.4 | 30.6 | 50.6 | 21.9 | 222 (71) | 4m47s |
| 332 | 4 pool-verify-v1 · 3.5 | 12.2 | 69.2 | 6.7 | 17.6 | 57.2 | 10.4 | 60 (19) | 3m28s |
- All five so far: **3 > 2 (16.0) > 1 (13.9) > 5 (13.5) > 4 (12.2).** Volume drives grounded F1, as R16 said: x2 nearly doubles findings and adds +5.3 F1. 3.5 is worse than 3 Super in both pipelines on ReviewBench.
- **Current P008 winner: candidate 3 (21.3).** Candidate 6 needs ≥ 24.3 to win (amendment 2's ≥ 3.0 margin). **v3 needs ≥ 24.3** (amendment 7).
- **Console: $68.09** (Sunny, 02:26; an earlier line said $60.09, which was wrong). Still to pay:
  - candidate 6 smoke + test, about $2.5;
  - the rest of relay 070, up to $14;
  - v3 smoke + test, about $3;
  - **one final**: x2 is about 2× pool-raw ≈ **$40** (unmeasured; v3 is estimated at about $33).
  - Worst case is about $60 against $68, so it fits with about $8 spare. Top up only if the final's measured cost per PR comes out higher. Ultra (amendment 6) likely won't fit.

## 23 Oct 23:00: test runs 3/6 done; candidate 6 uses 3 Super; cap $60 confirmed; #60 and #61 merged; relay on 070
- **#60 and #61 merged** (Sunny). **Relay running 070.** **The local total cap goes to $60** (Sunny confirmed 22:50).
- **ReviewBench test runs on `9b9a5127`** (25 PRs each; grounded F1 / P / R · augmented F1 / P / R · findings (novel) · time):

  | # | candidate | G F1 | G P | G R | A F1 | A P | A R | findings | avg time |
  |---|---|---|---|---|---|---|---|---|---|
  | 328 | 2 pool-raw · 3 Super | **16.0** | 70.6 | 9.0 | 23.0 | 52.5 | 14.7 | 124 (35) | 2m33s |
  | 329 | 5 pool-raw · 3.5 | 13.5 | 56.3 | 7.7 | 19.0 | 44.0 | 12.1 | 86 (22) | 3m08s |
  | 330 | 1 pool-verify-v1 · 3 Super | 13.9 | 79.5 | 7.6 | 22.3 | 67.2 | 13.4 | 99 (40) | 2m53s |

- **Candidate 6's model (amendment 2): 3 Super.** Candidate 2 (16.0) beats 5 (13.5) by 2.5, which is more than the 2.0 tie band.
  - Before its config is added: **one smoke of `pool-raw-missed` on 3 Super on `9b9a5127`**. Its earlier green smoke was on `ff22b972`; the 23 Oct lesson requires the exact image.
- On ReviewBench, **3.5 did worse than 3 Super** on pool-raw: lower precision (56 vs 71) and fewer findings, the opposite of dev. Single 25-PR runs; noted, not acted on.
- Remaining: candidate 4 (pool-verify-v1 · 3.5) + candidate 3 (pool-raw-x2 · 3 Super) now; then smoke 6, add config 6, run 6.
- **v3's bar (P008 amendment 7):** test grounded F1 ≥ the P008 winner + 3.0. Currently 16.0 + 3.0 = **19.0**.

## 23 Oct 22:55: Sunny has a Cursor premium subscription
- Uses (chat's proposal):
  1. **Bugbot in the 26 Oct seeded test** as the fourth tool (R13: Cursor's terms want a replication kit to publish results).
  2. **Bugbot as an extra independent reviewer on the 070 (recall-v3) PR**, standing in for the spent Ultrareviews. Its comments are input, not a gate.
  3. **A Cursor agent as the UI lane's builder**, working from the UI chat's spec: `viewer/` only, never `packages/` or `scripts/` (relay owns those tonight).
- Not for: anything touching the Action, the bench or ReviewBench code (one owner per path).

## 23 Oct 22:50: v3 recall mode: R16 research → P008 amendment 7 + P009 + relay brief 070
- **[R16](research/R16-reviewbench-grounded-f1.md)** (Sunny's Research chat):
  - recall is the lever;
  - the leaders win by "include nits", gates off, lenses and a "look once more" pass;
  - our self-imposed losses: the adapter caps at 40, drops null-line findings, and about 1/6 of chunks truncate;
  - Semgrep and "author next fix" rank low;
  - estimate (unmeasured): grounded F1 28–37 at about $0.05/PR (about $33 final).
  - **Risk: augmented precision** (ours 52.5 vs 80–92 for everyone on the board).
- **[P008 amendment 7](decisions/P008-reviewbench-preregistration.md):** still one final run. It goes to v3 only if P009 picks it. Disclosed: designed after seeing candidate 2's aggregate.
- **[P009](decisions/P009-reviewbench-v3-recall.md):**
  - `recall-v3`: per-file chunks; lens A (correctness, reliability, security, performance, API) + lens B (tests, docs, typos, maintainability, completeness) + `review-missed-v1`; T 1.0 / top_p 0.95; reasoning budget; reduce (anchor, dedup, cap 60, never fail).
  - **Dev:** arms S (3 Super) and V (3.5), one run each; floor = v1 P ≥ same-model pool-raw dev mean − 10; pick = higher v1 recall.
  - **ReviewBench:** smoke → one test run; final to v3 only if its test grounded F1 ≥ P008 winner + 3.0.
  - **Cutoff:** v3 test run by 25 Oct 23:59.
  - **Budget:** relay $14; local total cap $45 → $60 (Sunny to confirm).
- **[070 brief](prompts/2026-10-23/070-v3-recall-mode.md)** for relay, overnight; dev verdict by 24 Oct 12:00.

## 23 Oct 22:10: #60 (069b) and #61 cleared; results page waits for 26 Oct; v3 recall research started
- **#60 (069b intervals, relay, head `aec07e3`, CI green): cleared to merge.** 6 files, all in scope; `intervals.test.ts` 10/10 on Node 22; the relay reviewer recomputed the counts. **Allowed sentences (PR-level paired bootstrap, seed 69):**
  - **All 50, both judges:** v2 is behind Augment; **statistically tied with Cursor and Greptile**; ahead of Copilot and CodeRabbit.
  - **Cal.com:** tied with all except CodeRabbit (ahead; a 10-cluster bootstrap, so caveat it).
  - **vs 3 Super, all 50:** ahead under v1, tied under x1.
  - "3rd of 6" only as a point estimate.
  - The secondary (cc-01) method wasn't reproduced to 0.1, so it was skipped as pre-registered.
- **Relay's proposed 070 (results page now): NO.** It goes into the **26 Oct fill**: one results/README/viewer update with ReviewBench final, seeded test, 069 and 069b together. The UI redesign chat is editing the viewer, and two viewer edits would collide.
- **bunny-05:**
  - #54 head `1c57c72` (stays draft);
  - **#61** docs-only contracts security boundary (head `eef0604`): **cleared to merge**;
  - #58 head `ff483fc` (stays draft).
  - Open question from Bunny: "no network" in the write-up. The contracts stage can run a credential-free `git fetch` of the base commit, so the wording becomes "no model calls; network only for a credential-free git fetch of the base commit". Sunny to OK.
  - CLAUDE.md still says "read-only symlinks": the fill job fixes it.
- **Sunny wants grounded F1 to rise a lot (v3).** Chat's analysis:
  - The metric excludes unmatched findings, so **recall is the lever**: about 5 findings per PR against about 12 golden TPs.
  - The golden set includes Semgrep and author follow-up fixes.
  - The levers: per-file fan-out, diverse lenses unioned, sampling, static tools, a missed pass, truncation.
  - → [research-01](prompts/2026-10-23/research-01-reviewbench-grounded-f1.md) for a Research-mode chat.
  - **Integrity:** a v3 would be a new pre-registration (P009) designed *after* seeing candidate 2's aggregate score. Disclose it; never use `golden/`; validate on our dev set.
  - Constraint: money (a v3 final likely costs 2–4× pool-raw) and time (Orchestrate 24–25).

## 23 Oct 22:00: RB test run #328 (candidate 2, pool-raw / 3 Super): succeeded
- **Grounded F1 16.0 (P 70.6 / R 9.0); augmented F1 23.0 (P 52.5 / R 14.7).** 124 findings (35 novel) on 25 PRs, average 2m 33s per PR.
- For context (leaderboard, all 219 × 3, from R7): Greptile 27.2 (P 86.1 / R 16.2), Cursor 17.3 (P 87.7 / R 9.6), Copilot 40.1. A 25-PR test subset isn't directly comparable.
- **P008's expectation (recall 25–30%, F 38–44) was badly wrong.** Recall 9% at about 5 findings per PR, against about 12 golden TPs per PR. The metric is recall-driven, as P008 said, so the volume candidates (3 x2, 6 missed) matter most.
- Per P008: no tuning on this; agent output is read only for status and counts. Next: candidates 5 + 1 together.

## 23 Oct 21:55: P008 amendment 6 (report-only Ultra row); model-study and "beat Greptile/Cursor" answered
- Sunny asked for single test runs on Nemotron 3 Ultra, Claude Opus 5.5, Kimi K3 and Codex, plus architecture changes to get above Greptile and Cursor, and may buy Cursor for a month.
- **Amendment 6 written** (before any test result): one **report-only** Ultra row after the final, if Ultra is callable in the egress regions, smoke-green, and ≥ $15 is left. Opus, GPT and Kimi are out (egress locked, API keys, cost).
- **Chat's view on beating Greptile and Cursor:**
  - no product change before submission: freeze-2 is set, and Cal.com's one v2 look is spent, so nothing new can be validated blind;
  - we're already tied with both on Cal.com, and 069b gives the all-50 intervals;
  - the one lever with mechanistic evidence is **truncation**: 10/64 Cal.com 3.5 responses hit `max_tokens` 32k, and 7/40 dev PRs and 1/10 Cal.com PRs lost a chunk. It's a "what's next" item for after the hackathon, tested on dev.
- **Cursor for a month:** only useful for **Bugbot in the 26 Oct seeded test**. Bugbot draws from Pro's included usage, about $1–1.50 per run (third-party, Sep 2026), so 30 PRs could exceed the pool. Cursor's terms need a replication kit to publish results (R13).

## 23 Oct 21:35: RB test run #328 (candidate 2) running; 069b + bunny-05 written
- ReviewBench test run **#328** (pool-raw / 3 Super, `9b9a5127`) started 21:05:36. The others wait for it to succeed, then go **two at a time** (shared Nebius key; 429 risk).
- [069b](prompts/2026-10-23/069b-v2-intervals.md) (relay, $0): PR-level paired bootstrap (seed 69) for the v2 gaps on Cal.com and all-50, plus the cc-01 §3.2 method for continuity. The wording rule is fixed in advance: "ahead/behind" only if the primary 95% excludes 0.
- [bunny-05](prompts/2026-10-23/bunny-05-readme-and-docs-fixes.md): #54 README brought up to date except numbers (pin `@review-freeze-2`, no `model:` line, contracts merged, 067/068 wording, duplicate H1, Mermaid); new docs PR for the contracts security boundary (codex-13); #58's sentence fixed.
- Codex and Grok: nothing useful right now. Chat writes the 26 Oct seeded-test brief next.

## 23 Oct 21:05: portal configs 1–5 ready
- All 5 configs are on `9b9a5127317b…`, labels exact, credentials stored (Sunny's screenshot, checked by chat). Candidates 1 and 2 were edited in place (rows kept); 3, 4 and 5 were added. No runs yet. Next: test runs in order 2, 5, 1, 4, 3, then 6 (model per amendment 2).

## 23 Oct 20:55: #59 merged (main `5fb8cdc`); R15 UI references filed
- **#59 merged** → main **`5fb8cdc`** (merge commit, parents `8712273` + `7f757c1`; verified).
- **grok-11 → [R15](research/R15-ui-references.md):** 10 verified references plus lmarena. Patterns: real artefacts; honesty markers beside each claim; a summary that expands into detail.
- **UI redesign runs in a separate chat session** (Sunny). It's major on the frontend and small on the graph page. That chat should read R15, R14 (no competitor names in the video), `viewer/test/benchmark.test.tsx` (the `/benchmark` tables must equal `results.md`), and know the 26 Oct numbers fill. Merge by 27 Oct noon.

## 23 Oct 20:20: codex-13 audit (partial; OpenAI's cyber filter cut it off), so no freeze break
- The **OpenAI cyber filter blocked Codex** mid-audit. Sunny had it stop and report. [Report](prompts/2026-10-23/codex-13-hostile-pr-audit.out.md).
- **Verified:**
  - **Q1 issue:** a PR's tsconfig `extends` (relative or absolute) and `paths` make the compiler read files **outside the checkout**. A fixture's external `.d.ts` literal reached a finding's text (TS2345 in an unedited file), so it could land in the posted contracts section (`typed.ts:114/163`, `findings.ts:44`, Action `contracts.ts:185`). Arbitrary credential-file disclosure was **not** shown: the content has to type-check into a diagnostic.
  - **Q2 / Q9:** tsconfig `plugins`, `typeRoots`/`types` and package `extends` were read as JSON or `.d.ts`; **no PR JS was executed** (`PLUGIN_EXECUTED False`).
  - **Q6, partly:** the child env strips `GITHUB_TOKEN`, `GH_TOKEN`, `NEBIUS_API_KEY` and askpass, but keeps `ACTIONS_*`.
- **Unchecked:** Q3 symlinks, Q4 size/hang, Q5 text injection, Q7 git filters/submodules, Q8 (node_modules symlinks aren't actually read-only).
- **Chat's call: no freeze break; backlog plus docs.** Threat model: the Action reviews **same-repo PRs only** (forks skipped in code, `action.ts`). A same-repo PR author has write access, and on `pull_request` their branch's workflow file runs with the repo's secrets, so they can already read the runner's files and secrets directly. Q1 doesn't cross a trust boundary the Action promises. It *would* matter if fork PRs were ever reviewed, so the fork skip must stay hard-coded.
  - **Docs, in the 26 Oct fill (docs-only, allowed after freeze):**
    - `docs/action/README.md:171` and Devpost `writeup.md` change to "doesn't execute PR code or tsserver plugins; it parses the PR's config and type files, which can reference files outside the checkout. Only same-repo PRs are reviewed; forks are skipped." No "no script" absolute, and no sandbox implication.
    - `docs/contracts.md:15`: node_modules symlinks aren't enforced read-only.
  - **Backlog (post-hackathon):** an allowed-root read policy for compiler/config reads (realpaths, symlinks included); an env allowlist for the analyzer child (drop `ACTIONS_*`); finish Q3–Q8 in a Claude lane.
  - Not re-running the rest now: Bunny's time goes to UI/Devpost; the residual risk is inside the existing trust boundary.

## 23 Oct 20:05: 069 DONE (v2 Cal.com, all 3 predictions right); #59 and #58 checked; codex-13 stopped on flakes → 13b
- **069 / #59** (relay, head `7f757c1`, CI green): **cleared to merge** (merge commit). Chat checked:
  - the diff is only `scripts/bench/run-test-set.ts`, `test-set.ts`, `test-set-run.test.ts` and docs; `packages/analyzer/src` and `config` are identical to `review-freeze-2`;
  - gate 069 + model check run before any call; runs 1–3 are pinned;
  - `test-set-run.test.ts` 50/50 on Node 22;
  - headline and all-50 P/R/F recomputed from the counts, all match.
  - Pre-registration `b2ec214` was pushed 13:38:03Z, 48 s before the first Cal.com 3.5 call. [Report](prompts/2026-10-23/069-v2-calcom.out.md).
- **v2 result (Cal.com run 4, n = 23, one run, second look):**
  - pool-verify-v1 on 3.5: **v1 P 63.2 / R 52.2 / F 57.1**, x1 F 58.5. 3 Super run 1 was F 58.3 (runs 1–3: 51.3 / 52.8 / 58.3).
  - Predictions (a) F ≥ 52.8 ✔, (b) P ≥ 55.6 ✔, (c) ≤ 3/10 failed-chunk PRs ✔ (1/10: PR 4, both reviewers invalid JSON, scored 0).
  - 10 of 64 responses hit 32k tokens. $0.7285; **ledger $34.53 / $45**.
  - **All-50 v2 (dev 7r + Cal.com 4): v1 F 52.2, x1 F 55.4, 3rd of 6 under both judges as a point estimate** (Greptile 49.5 / 55.1; x1 gap 0.3). No bootstrap was done, so **no public ranking claim**: public wording is decided with the 26 Oct numbers. Precision up, recall down vs 3 Super, the same pattern as dev.
  - Brief error (chat's): the Action's `max-comments` default is **15**, not 10 (the dogfood sets 10). Relay reported both; irrelevant here (no PR > 10 kept).
- **bunny-04 / #58** (head `3d5a62d`, docs only, 5 files in `docs/devpost/`): **fine, keep it a draft** until the 26–27 Oct fill. No competitor names, "beats" or "3rd" in the text. `writeup.md`'s "no script from the pull request is run" must be reworded in the fill (see 20:20 entry). [Report](prompts/2026-10-23/bunny-04-devpost-drafts.out.md) has 12 `[[SUNNY]]` questions. Also found: the repo About description is pre-pivot, with empty topics (Sunny fixes it by hand); #54's install block is stale after 068 (fill job).
- **codex-13** stopped at Step 0: two 5 s test timeouts under load, nothing audited ([out](prompts/2026-10-23/codex-13-hostile-pr-audit.out-1-stopped.md)). → [codex-13b](prompts/2026-10-23/codex-13b-hostile-pr-audit-resume.md): timeout-only failures get a 30 s rerun of that file, then continue; adds Q9 (is "no PR script runs" true?).

## 23 Oct 17:15: #57 MERGED (main `0aeef06`), ReviewBench #92 approved; today's hand steps written
- **#57 merged** 16:25 IST → main **`0aeef06fa8742cc8a36581c3894e554b8748c965`** (merge commit, parents `8d0fb7a` + `4e3a0a8`). CI on main green. No ReviewBench image built from it yet.
- **ReviewBench #92 merged** 22 Oct (maintainer dapirian approved; bot: "start a test run in the vendor portal"). Onboarding done.
- Adapter check (for amendment 5): `pool-verify-v1` in `scripts/reviewbench/entry.ts` calls the Action's `runPipeline`, which is partial since #57; pool-raw / x2 / missed keep successful chunks (`reportPass` fails only when every chunk of the pass failed). So the post-#57 image applies the partial rule to **all four pipelines**.
- **P008 amendment 5: APPROVED (Sunny 17:11) and written** to P008 at 17:20, before any test run. Image for all candidates: **`sha256:9b9a5127317be79f4bc9f7e771b84b441d887f5cdd52e38e934b8657ceb84508`** (ReviewBench image run #8, from `0aeef06`, adapter 70/70). Adapter-relevant diff vs `ff22b972`: only pipeline.ts, entry.ts, client.ts. Verifier also runs on `RB_CONFIG_MODEL` (as in the Action and 067).
- **Smoke before portal (chat, on rethink):** pool-verify-v1 on 3.5 (candidate 4) was never smoked on any image, and pool-raw-x2 (candidate 3) was never smoked at all. Three smokes on `9b9a5127`: pool-raw/3.5, pool-verify-v1/3.5, pool-raw-x2/3 Super.
- **Freeze-2 moved to today** (Orchestrate takes 24 Oct 18:00 → 25 Oct 18:00). Hand steps for Sunny: [sunny-today-freeze-2](prompts/2026-10-23/sunny-today-freeze-2.md): top-up → image rebuild → 3.5 smoke → portal configs 1–5 (6 after 2 vs 5) → tag `review-freeze-2` on `0aeef06` → re-pin dogfood + demo to `0aeef06` **with explicit `model: nvidia/Nemotron-3.5-Super-VL`** → close/reopen demo PR #2 to check a live 3.5 review.
- Relay: Sunny says done; nothing pending there.
- **Parallel lanes started (19:20):** [codex-13](prompts/2026-10-23/codex-13-hostile-pr-audit.md) read-only hostile-PR audit of contracts (report only); [bunny-04](prompts/2026-10-23/bunny-04-devpost-drafts.md) Devpost drafts in `docs/devpost/` with placeholders (draft PR); [grok-11](prompts/2026-10-23/grok-11-ui-references.md) UI references for the redesign. **UI redesign:** major on the frontend, small on the graph page (Sunny 19:11). Brief waits for Sunny's specifics (pages, what's wrong, references, must-not-change); lane proposed Codex from 24 Oct after codex-13; merged by 27 Oct noon.
- **[069 v2 blind Cal.com brief](prompts/2026-10-23/069-v2-calcom.md) written (19:10):** relay, cap $3, gate `TRACEHOUND_TEST_RUN=069`, Cal.com run 4 on 3.5 from `review-freeze-2`, one run (second look), predictions (a) v1 F ≥ 52.8, (b) v1 P ≥ 55.6, (c) ≤ 3/10 failed-chunk PRs, all-50 v2 = run 4 + dev 7r. Must start before 24 Oct 18:00.
- **Tag `review-freeze-2` pushed (18:11):** annotated tag object `6d38b35`, peels to **`0aeef06`** (verified on GitHub). **Re-pins done and verified (18:17):** TraceHound main `8712273` and tracehound-demo main `88f6721` each change only `tracehound-review.yml`: pin `@0aeef06…` + `model: 'nvidia/Nemotron-3.5-Super-VL'`. TraceHound main is now `8712273` (the tag stays on `0aeef06`). Demo PR #2 close/reopen (18:2x): run #27 ([39957425049](https://github.com/SunnyBagal/tracehound-demo/actions/runs/39957425049)) green, job 17 s, **but no new review posted** (API: last review still 18 Oct, Nano header). The Action exits green without posting on: 0 findings, reviewer failed (both reviewers every chunk), stale head. **Log (Sunny, 18:48): `TraceHound: reviewer returned no findings; nothing posted.`** (step 5 s, TRACEHOUND_MODEL 3.5, contracts on, learn off.) So 3.5 at freeze-2 misses #2's seeded line; Nano generators caught it on 17/18 Oct. **Chat's call: no change** (1 PR vs 067's 40-PR dev result; the seeded test on 26 Oct gives the real rate and the demo reviews; no more reopening demo PRs). README fill must not present the old 2-of-4 demo stats as the current config. Backlog: the no-post log line should carry pool/kept counts. If it's 0 findings, the demo's seeded catch on #2 is lost under 3.5: a judges' demo problem, decide before the seeded test.
- **Smokes on `9b9a5127` (17:49), all green:** pool-raw/3.5 idx0 ok 1f $0.1148 150 s, idx4 ok 1f $0.0102 13 s; pool-verify-v1/3.5 idx0 **ok partial=1/2** 0f $0.0926 171 s, idx4 ok 1f $0.0097 15 s; pool-raw-x2/3 Super idx0 ok 2f $0.0848 308 s, idx4 ok 1f $0.0086 27 s. ~$0.32 (CI spend, outside the local ledger). Partial rule confirmed live. → [portal + test-run order](prompts/2026-10-23/sunny-reviewbench-test-runs.md): configs 1–5 on `9b9a5127`, run order 2, 5, 1, 4, 3, then 6.
- **Top-up done (17:06): Token Factory console $70.69.** Portal shows TraceHound `approved`, test run page live, only the old `ff22b972` configs listed, no runs yet.

## 23 Oct 16:40 (Sunny was unwell 22 Oct; no hand steps done that day): #56 merged (main `8d0fb7a`); #57 CLEARED; handoff Oct23
- **#57** (codex-12, decision 068, head `4e3a0a8`): **cleared to merge.** Checked against the source diff:
  - partial results in `runPipeline`; it fails only when both reviewers fail every chunk;
  - the footer field `chunkFailed {diff, ctx}`;
  - `...pipeline` replaced by explicit `pool`/`kept`, so the key order and bytes are unchanged when nothing fails;
  - `review-diff-v1` keeps all-or-nothing;
  - the `model` default is 3.5; `FIX_MODEL` is unchanged;
  - the adapter's pool-raw / x2 / missed keep successful chunks, and a pass fails only if all its chunks failed;
  - the smoke regex is `^ok( partial=N/M)?$`.

  Action 237/237, adapter 70/70, adapter tsc clean (Node 22). [Report](prompts/2026-10-22/codex-12-068-default-3.5.out.md).
- **Sunny:** top up about **$50** on the Nebius console (decided). P008 amendment 5 is still unanswered. Next: merge #57 → rebuild image → 3.5 smoke → re-add portal configs on the new digest → freeze-2 (24 Oct, tag `review-freeze-2`, re-pin demo and dogfood).

## 22 Oct 04:15: 067 says ADOPT 3.5, but the Action doesn't match what was measured
- **#56** (relay 067b, head `46a678d`, CI green): **cleared to merge.** Chat recomputed every P/R/F from the TP/FP/FN columns (7r v1 50.72, 8 v1 50.00, mean 50.4; x1 54.41/55.88 → 55.1). The rule passes on 2 runs (run 9 was cut by the cap, so the fallback applies). Bootstrap v1 +8.1 [+1.4, +14.4]; x1 +3.9 [−4.2, +11.1] includes 0. Both 3.5 runs beat every one of the five 3 Super runs. The gain is precision (half the FPs). Cost per PR about 2× (median $0.063). Spend $7.98; **ledger $33.81 / $45**.
- **Catch (chat):** the bench scores a PR with a `CHUNK_FAILED` chunk **with partial results** (`review.ts:5`), and 3.5 failed a chunk on 7/40 PRs per run. The **Action** (`pipeline.ts`: throw on `chunkFailed > 0`) and the **ReviewBench adapter** (all-or-nothing) post **nothing** in that case. Switching the default alone would ship an unmeasured config that is silent on about 1 in 6 PRs. → [codex-12 / decision 068](prompts/2026-10-22/codex-12-068-default-3.5.md):
  - partial results in the Action and the adapter;
  - `model` input default → 3.5;
  - fixes stay on Super;
  - byte-identical when nothing fails.

  It must land before freeze-2 (24 Oct).
- **P008 amendment 5 (proposed, Sunny to approve, before any test run):** candidates run on the image with 068's partial rule (all candidates alike). Without it, 3.5 candidates 4/5 lose about 17% of PRs outright on ReviewBench, under a rule that 067 didn't use.
- **Relay's 3 questions, answered:**
  1. Merge #56: yes.
  2. The default switch goes to Codex (owns the Action), not relay's proposed 068.
  3. Relay Nebius cap goes to **$0** until the v2 Cal.com run brief (about 25 Oct), which gets its own cap.
- **Money:** 3.5 doubles cost. ReviewBench final on 3.5 pool-verify ≈ 657 × $0.063 ≈ **$41** (was $21). That's on the separate `reviewbench` key, so the **console balance needs a bigger top-up** before the final.

## 21 Oct 21:32: #55 merged → main `da13585`; image `bc6530f2`
- **ReviewBench image with 3.5 routing:** `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:bc6530f2d2fc9379a55f466be9917b8e9bf51dd5816fa90db0c8f759bbe6d0eb`, built from `da13585` (#55 merge). Adapter 47/47, public. Use it for smoke run 4 (3.5) and candidates 3–6.
- Smoke on `bc6530f2` (21:42) ran **3 Super by mistake** (model input left at default): pool-raw green, idx 0 2 findings $0.0422 175 s, idx 4 1 finding $0.0044 27 s. That confirms #55 leaves 3 Super unchanged. **Smoke 3.5 (21:54), pool-raw on `bc6530f2`: RED.** The routing works: the calls reached 3.5. idx 4 ok, 1 finding, $0.008, 12 s. idx 0 `failed: review: failed chunks (first: the reply is not valid JSON; after one retry: the reply is not valid JSON)`, 0 findings, $0.0957, 159 s. Likely reasoning using up `max_tokens` 32k (unverified, no `finish_reason` kept). That's model behaviour, so it doesn't trigger P008 amendment 4 (3.5 is callable). → [067b](prompts/2026-10-21/067b-resume-dev-3.5.md): resume 067 on dev with a pre-registered stop-rule amendment (failed chunks count as the result; stop if more than 10/40 PRs fail or on any request error). Verdict by 23 Oct.

## 21 Oct 21:00: #55 (bunny-03) CLEARED, CI green (21:07): merge it
- **#55** `fix/model-base-url` head `5ec764d`, 4 commits. Checked by chat:
  - `baseUrlFor` rewrites only Token Factory hosts (`^api\.tokenfactory\.[a-z0-9-]+\.nebius\.com$`) and only exact table ids (`Object.hasOwn`).
  - Probes: a lowercase id, 3 Super, a look-alike attacker host, localhost, `__proto__`/`toString`, and a non-URL all stay unchanged.
  - The cache key is sha256(model + request), with no URL, so it's unchanged.
  - Analyzer guardrail tests 25/25, adapter 47/47, adapter tsc clean (Node 22).
  - The FEEDBACK entry sticks to facts.
  
  **Merge when CI is green**, then rebuild the ReviewBench image → smoke run 4 (pool-raw on 3.5) → the 067 resume prompt. Bunny's report is filed ([out](prompts/2026-10-21/bunny-03-model-base-url.out.md)): workspace tests action 226 / analyzer 869+7 / viewer 188, adapter 47; a 3 Super cache-key pin test was added.

## 21 Oct 20:35: bunny-03 written
- Image rebuilt on `7de5dbd` (20:52): `sha256:8e1ec5f267a02e8448a32309e1a054f128b93b2d0ce1efe0ea80ae7ae85b166d`. It was built **before** bunny-03, so it has no 3.5 routing. ReviewBench behaviour is the same as `ff22b972` (#53 touches only the Action). Don't use it for 3.5, and don't register it.
- [bunny-03](prompts/2026-10-21/bunny-03-model-base-url.md): per-model base URL in the client (3.5 → us-north1), adapter log, docs, FEEDBACK entry. Draft PR. After merge, Sunny rebuilds the ReviewBench image → smoke run 4 (pool-raw on 3.5) → the chat writes the 067 resume prompt (relay). 067 must run by 23 Oct.

## 21 Oct 17:10: #53 CLEARED → **merged, main `7de5dbd`** (verified 20:30)
- **#53 head `3b196fa`: merge it** (mark ready, merge commit). codex-11 ([report](prompts/2026-10-21/codex-11-int-01-fixes.out.md)): 7 commits, one per Ultrareview finding. The chat read the source diff against ur-02:
  - SIGTERM, then SIGKILL after 5 s (Node's own timeout is off; a timer of its own sends the kills). `shallow.lock` is removed only after our own SIGKILL, and only if it didn't exist before.
  - The fetch needs at least 30 s left. The analyzer gets the remaining time minus 10 s, and is skipped below 5 s.
  - A typed check skipped for budget or error keeps the keyed findings, with a `typed` footer field.
  - The section comes after the disclaimer; with `contracts` undefined the order is unchanged.
  - `escapeContractText` covers `& < > [ ] \` and backticks, and breaks `@`.
  - No checkout (or `rev-parse --git-dir` failing) gives status `off` and no delivery, so output is byte-identical.
  - Docs say "always body findings".
  - Action tests **226/226 locally (Node 22)**. The demo branch is pinned to `3b196fa` (`303b689`), trigger commit `71c5f49`. Section-after-disclaimer is checked in code and tests; WebFetch only rendered the old 20 Oct review.
- **Residual, backlog (analyzer lane):** `--budget-s` bounds only the **typed** check (`computeContracts`), not keyed or dependents. If those run long, the outer SIGTERM still lands. The Node analyzer has no SIGTERM handler, so its `finally` (closing the worktrees) doesn't run, and worktrees can still leak in that case. That's rare and cosmetic; the fix needs a change in `packages/analyzer`.
- **Next for freeze-2 (24 Oct, tag `review-freeze-2`):** re-pin the demo `main` and dogfood workflows to the merge commit **with `model: nvidia/nemotron-3-super-120b-a12b`**. Demo PR #6's review still lists Nano generators.

## 21 Oct 00:10: HOLD #53 → codex-11; smoke runs on 3 Super green
- **HOLD #53.** Ultrareview #2 ([output](prompts/2026-10-20/ur-02-int-01.out.md)) found 7 issues. The chat checked all 7 against `747a3a7` and confirmed them all. Fix round [codex-11](prompts/2026-10-21/codex-11-int-01-fixes.md) runs on the same branch. Findings:
  1. SIGKILL can leave `.git/shallow.lock`, which breaks a data-branch step in the same job.
  2. `--budget-s 90` doesn't subtract the time already spent, so worktrees leak.
  3. A typed check skipped for budget (more than 5,000 root files) drops the keyed findings.
  4. The contracts section sits before the disclaimer and gets swallowed by `learn` `bodyFindings`.
  5. Unescaped reason text: `<User>` is stripped and `@team` pings that team.
  6. With no checkout, a review-diff-v1 review now gains a footer.
  7. The inline path is unreachable, because the analyzer reports only unedited files. That's a docs fix.
- **Not covered by anyone yet:** a hostile tsconfig (`extends`, `plugins`) or symlinks in the PR. The focus text never reached the Ultrareview. Risk is low: fork PRs are skipped, so the PR author already has write access. Backlog.
- **Ultrareviews: 1 left** (reserve).
- **ReviewBench portal (21 Oct 00:07): registered**, identifier `tracehound`, awaiting approval (a review-request PR on their repo). Config 1 = `pipeline=pool-raw`, `model=nvidia/nemotron-3-super-120b-a12b`, digest `ff22b972…`. Credentials are per configuration. The portal page says **"ReviewBench provides the evaluation judge"**, so test runs may not need our Anthropic key, unlike R12. Confirm from the credentials form (only `NEBIUS_API_KEY` expected). If so, amendment 3's fallback is moot. **Credentials form asked only for `NEBIUS_API_KEY`** (Sunny used a separate `reviewbench` Nebius key, stored for config 1). So ReviewBench pays the judge on test runs too: **no Anthropic key needed, and P008 amendment 3's fallback is moot** (all candidates can test-run). R12's "self-funded" was wrong. Configs (00:28): **candidate 2** `pool-raw` and **candidate 1** `pool-verify-v1`, both 3 Super on `ff22b972`, labels exactly `pipeline` + `model`, credentials stored. Candidate 3 (`pool-raw-x2`) can be added the same way; 4–6 wait for the 3.5 fix. Onboarding PR **review-bench/ReviewBench#92** (open, checks green, awaiting code-owner review). Manifest verified from Sunny's screenshot: full digest `…e1276b23ba` (public), 1 env secret, Model API us-central1, **egress us-north1 + us-central1**, 1800 s.
- **Smoke runs on `ff22b972`, all on 3 Super, all green:**

  | pipeline | idx 0 (TS) | idx 4 (Py) |
  |---|---|---|
  | pool-raw | 2 findings, $0.0586, 284 s | 1, $0.0040, 16 s |
  | pool-verify-v1 | 0, $0.0436, 208 s | 1, $0.0047, 39 s |
  | pool-raw-missed | 1 (pool 0, missed 1, new 1), $0.1021, 607 s | 2 (pool 1, missed 1, new 1), $0.0066, 28 s |

  About $0.22 of CI spend, outside the local ledger. idx 0's pool varies between runs (2 vs 0 at temperature 0), the known spread. pool-raw-missed took 10 min on idx 0, against the 25-min limit. The 3.5 smoke run waits for the per-model base URL fix.

## 20 Oct 23:00: image rebuilt on main; Codex extended
- **ReviewBench image on main `2a95756`** (includes #51): `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:ff22b972feb243d7c4f73db69ced12ee4a811e66f552a094e77794e1276b23ba`. Public; adapter tests 46/46. Warnings are harmless (Node 20 deprecation, the `FROM --platform` lint). Use it for smoke runs 1–3 and the portal registration. The per-model base-URL fix will produce a newer digest, which is added in the portal with no re-approval.
- **Smoke runs on `ff22b972`:** #1 failed at the image check (a stray space in the input; $0). **#2 pool-raw / 3 Super: green.** idx 0 (TS): ok, 2 findings, $0.0586, 284 s; idx 4 (Py): ok, 1 finding, $0.0040, 16 s. pool-verify-v1 and pool-raw-missed are next.
- **Codex lane extended:** Sunny has ChatGPT Plus for 1 month (free). Codex no longer ends 22 Oct.

## 20 Oct 22:40: 3.5 found. It is on us-north1
- **Nemotron 3.5 Super VL answers on `https://api.tokenfactory.us-north1.nebius.com/v1`** with id `nvidia/Nemotron-3.5-Super-VL` (Sunny's curl, 22:37: 200, reasoning on). **3 Super is not on us-north1** (`model_not_found`), and 3.5 is not callable on us-central1, even though us-central1's `/v1/models` lists it (Nebius bug, FEEDBACK).
- **Effect:** the client has one global base URL (`DEFAULT_BASE_URL` us-central1, or `NEBIUS_BASE_URL`), so no run can mix the two. 067 needs 3.5 reviewers plus Super/Qwen judges. ReviewBench candidates 4/5 need 3.5, and 1–3 need 3 Super.
- **Fix (proposed):** a per-model base URL in the client (3.5 → us-north1, else the default), used by every stage and the ReviewBench adapter; the smoke workflow passes it through. No network in tests. Then 067 resumes and smoke run 4 runs.
- **Portal:** register with Model API URL **us-central1** plus other egress host **`api.tokenfactory.us-north1.nebius.com`**. Declare it now, because the docs don't say egress hosts can be edited later.

## 20 Oct 21:40: #53 and #54 checked; #52 merged
- **main `2a95756`** (#52 merged, verified). #42 closed unmerged by Sunny (21:33).
- **Ultrareview #2 on #53: run it** ([ur-02](prompts/2026-10-20/ur-02-int-01.md), new cloud session). Then **1 Ultrareview left**, kept in reserve for its fix round or an emergency. No later PR before submit is bigger: #53 is the last product code that runs on untrusted PR code by default.
- **#53 int-01 (Codex), head `747a3a7`: chat check passed, no HOLD.** Merge after Ultrareview #2 (cloud). Run it **tonight**: Codex ends 22 Oct, so a fix round has to start tomorrow.
  - Verified: 13 files, all in lane; Action tests 214/214 on Node 22. `contracts: 'off'` is byte-identical: hard-coded legacy payloads for both reviewers and both fixes modes, and no off-path change in the diff. It runs concurrently and uses the merge base. It is fail-soft with one log line and doesn't apply `learn`. Inline is capped at 10, outside `max-comments`. The ±3 dedupe keeps the contract finding. Credentials are stripped (tokens, askpass, `extraheader` reset for the fetch).
  - Live: demo PR #6 is pinned to `747a3a7`. Its review body lists 4 dependents (`index.ts:13`, `scripts/repair-blocked-metadata.ts:22`, `test/linkDetector.test.ts:4`, `worker.ts:10`, all TS2305). The 2.255 s is in the hidden footer and wasn't checked here. Demo PR #6 must never merge.
  - Backlog (not blocking):
    1. The dedupe matches against every contract finding, including ones shown only as "and N more", so a model finding can vanish while its contract is hidden. This needs more than 10 body findings.
    2. The body's reason text isn't Markdown-escaped (the path label is). Route, env and event keys come from PR code, so a PR can inject links or @mentions into the bot's review.
    3. A typed check skipped for budget or error marks the whole stage failed and drops the keyed findings too, so big repos always report failed.
    4. The review checkout is depth 1, so every run takes the credential-free unshallow fetch. Private repos fail soft (documented: `fetch-depth: 0`).
    5. The footer has no `status: "off"` when off. That's needed for byte-identical output, and it's documented. Accepted.
- **#54 bunny-02 judges' README, head `db83286`: fine, but leave it a draft.** Merging now would put `[[RESULTS-TABLE]]` placeholders on the public README. The fill job (26 Oct) must:
  - change the try-it pin from `@main` to the freeze-2 tag;
  - drop "landing in int-01 (draft PR #53)" once #53 merges;
  - drop the 3.5 line if 067 is dropped;
  - remove the second `# TraceHound` H1 in Details;
  - check the Mermaid render.
  - Wording passes: no "beats", no "3rd of 6".
- **Demo and dogfood don't run the benchmarked model.** Neither workflow sets `model`: the demo pins `41a92a8` on main, and PR #6 and dogfood pin `e4e3e3a`. So both generators run on **Nano** (`DEFAULT_MODEL`), and only the verifier and fixes use Super. Demo #6's review header lists both models. The benchmark is Super. Bunny caught this in the README.
  - **Fix at freeze-2:** re-pin the demo and dogfood workflows with `model: nvidia/nemotron-3-super-120b-a12b`. No code change, no freeze risk.
  - **The seeded test (25 Oct) must set it too**, or it measures Nano generators.
  - The alternative, making Super the Action default, is a code change for Codex by 22 Oct. Sunny decides; chat recommends the workflow fix.
- **Portal: the Model API URL is locked after registration** (only a maintainer can change it, via the onboarding PR; ReviewBench ONBOARDING.md step 5). Sunny confirms 3.5's base URL in the playground code sample **before** registering. If it isn't `https://api.tokenfactory.us-central1.nebius.com/v1`, the chat decides the URL first. If 3.5's model id differs from `nvidia/Nemotron-3.5-Super-VL`, the smoke workflow's `model` choice list needs a one-line PR.
- **Tag name:** `eval-freeze-2` already exists (4 Oct, repair eval). The 24 Oct tag is **`review-freeze-2`**.

## Repo state

- **TraceHound main: **`b98f753`** (verified 19 Oct 13:55, #45 merge). Earlier: `5857179` (#47), `a9d16b5` (#46), `2af8c3a` (#43).** Before that `c6c12b9` (#44 merge). Older: main `e7309a5` (verified 18 Oct 00:10): #33 merge `6b4276e`, #34 merge `e7309a5`. Tag `review-freeze-1` → `6b4276e` (#33's merge commit), tagged 17 Oct 22:14 IST. #35 merged 18 Oct (`3113c3a`). #36 → `d37ffa8`, #32 → `e95ef20`, #37 → `df4f915` (all merged 18 Oct). #38 → `41a92a8` and #39 → `6058d53` (both merged by Sunny 18 Oct ~20:25, merge commits; #38 before codex-06c, by accident; harmless: suggestions work, #2's just isn't committable). #40 merged (`e4e3e3a`).
  Open:
  - **#43** codex-07 data branch, head **`19ece32`** (12 commits; ur-01 fixes on top of `d733a6a`). **Re-checked by chat 19 Oct 00:10:** source diff read against ur-01; #1 merge-base diff, #2 monotonic latest + state rank, #3 ref-lock retry, #4/#8 split `git`/`branchGit` from `cleanGitEnv()`, #9/#10 last-line footer + JSON temp file all match; #5 backlog line; #6/#7 example split into review (cancel-in-progress) and data (serialized, only `contents: write`) jobs. Action tests 153/153 and typecheck re-run locally; merges cleanly with main `c6c12b9`. `max-comments: '0'` returns before any GitHub/Nebius call (verified in `action.ts`). Accepted departures: reopened → open; #5 own commit; extra re-run row. **Merged 19 Oct** (`2af8c3a`). Residuals, backlog only: a manual re-run of an old `reopened` after a later close flips the entry back to open; a late event for a new head after close adds an open entry; dropped pending data jobs can leave a closed PR `open` (documented); the split example records no `reviewFooter` (viewer doesn't read it today; ui-03 must not depend on it). Dogfood pin still `e4e3e3a`.
  - **#44** ui-02 dashboard: **merged** (main `c6c12b9`, verified 19 Oct 00:05). Re-sync `viewer/lib/benchmark.ts` after the R6 wording fixes land (it still shows "3rd").
  - #41 (cc-01 audit) and #42 (ReviewBench spike) are drafts; their content feeds later jobs. Next relay: **064 voting across runs** (`064-vote.md`, cap $10, dev only, PR as draft). Next Codex: `codex-06c-deleted-lines.md` (rewritten against main, new branch `codex/fix-committable`).
- Recall main `20ac149`, `testable-baseline` `57d920e`. Never open Recall PRs #1 or #2.
- Nebius: **console balance $39.93** (18 Oct 21:00; was $46.22 on 17 Oct; −$6.29 = 062's $5.31 + about $0.98 of off-ledger CI: dogfood + demo live checks); ledger **$23.17092** after 064 (19 Oct; was $12.45236). Caps: total stays at the $45 default (ledger + $15 fits; decided 17 Oct), per-job `nebiusCapUsd` → 15 for 062 ([costs](ops/costs.md)).

## In flight

#33 and #34 merged with merge commits: **verified on GitHub 18 Oct**.

- **#35** `ui/viewer-round-1` head `acc5752`, 18 commits: checked by chat 18 Oct. Lane paths only; $0 Nebius; no deps; snapshots at Recall `20ac149` and demo `efc5844` = their mains; CI green (the prisma annotation comes from the analyzer fixture and was already there). One red intermediate commit (`f6df5a9`), disclosed. Nits go to ui-02: edge labels collide, titles truncate, "1 modules". Step 9 now goes to a **new branch** via `ui-01b-after-062.md`.
- **062 done → PR #36** (head `9c0624f`, CI green). Checked by chat 18 Oct 03:40:
  - all 16 table rows recompute, and the spreads are consistent;
  - frozen paths are unchanged against `review-freeze-1`;
  - the predictions match 061's wording;
  - no Cal.com text.
  - Relay's BLOCKED was only "main moved" (#35), which is harmless. `tracehound/025` is relay's own job counter (brief 062).
  - **Headline: blind Cal.com run 1, v1: pool-verify-v1 F 58.3% (P 56.0 / R 60.9), 3rd of 6** (augment 73.2, cursor 66.7 | greptile 50.0, copilot 46.2, coderabbit 36.7). It was 3rd in 5 of 6 run×judge cells and 5th in x1 run 3 (47.3).
  - Predictions: (a) right, (b) wrong, (c) right.
  - Dev spread: 35.6 → 48.0 across two runs (larger than the 0.6-point adoption margin).
  - Dev run 2 was void (relay's 2 h background limit killed it), then 2r; dev run 3 was stopped at the cap.
  - Spend $5.31; **ledger $17.76**.
- 063 → PR #39 (relay 026). Checked by chat 18 Oct: 4 small commits; ranks, spreads and costs trace to the repo.
  - Wording nit for the judges' README job: section 1 says x1 "changes nothing". It means it decides nothing; x1 reverses (a) and gives a 5th in run 3, both stated in §3/5/6.
  - Relay lane is idle until the judges' README (21–22 Oct).
- **Dogfood spend is off the local ledger** (`secrets.NEBIUS_API_KEY` in CI), roughly $0.03–0.25 per PR. Decided by chat: keep it on (it's live demo evidence) and track it via the Nebius console. Any future job that needs "no other Nebius use" opens its PRs as drafts (the workflow skips drafts).

| Lane | Job | Status |
|---|---|---|
| Relay | **062** Cal.com run (`briefs/062-test-run.md`, cap $15): fetches → tag `review-freeze-1` on #33's merge commit → Cal.com run 1 (headline) → runs 2–3 → dev runs 2–3 → results doc | Starting 17 Oct night. Needs `nebiusCapUsd` = 15, #32 unmerged, no other Nebius use. Takes hours |
| UI lane | **ui-01-codex.md**: round 1 + addendum merged, now **Codex GPT Sol 6.1** in ~/Projects/TraceHound-ui, branch `ui/viewer-round-1` | Starting 17 Oct night. No Nebius until Sunny writes "062 is done"; step 9 (Super summaries, $0.20) comes after |
| Codex | #32 dogfood workflow + demo doc | **Hold until 062's PR is done** |
| Codex queue | codex-06 fix suggestions → codex-07 data branch (persistence) | after 062 (its Action runs use Nebius) |
| Grok | grok-04 X post draft | Post Mon 19 Oct; Cal.com numbers only once 062's results doc has merged and the chat has checked it |

## Known problems

- **Codex automatic review** has posted nothing on #27–#31. Check the Review trigger, or comment `@codex review`.
- **Demo (17 Oct, pin `50e1d4a`):** the reviewer commented on the seeded line in 2 of 4 demo PRs (#2, #3), missed #1 and #4, and posted 0 comments on the control (#5). Dogfood #32: 1 comment.
- **No live reviewer demo yet**, and judges may score from the video and text alone (R3).
- **X post planned for Monday 19 Oct** (benchmark thread; tags to verify).

## Schedule

| Date | Benchmark lane (relay) | Product lane (Codex) |
|---|---|---|
| 16 Oct | 059 done (adopted) | #28 merged |
| 17–18 Oct | 060 scorer fairness: augmented precision plus a second judge | Demo repo with the action installed and real review comments (the judges' working demo); switch the action to the reviewer that wins |
| 20 Oct | Results doc (moved earlier) | Landing page section linking the demo and the benchmark tables |
| 21–22 Oct | Spare for the weakest stage | README for judges |
| 23 Oct | Freeze (tag), then the one Cal.com run | |
| 24–25 Oct | Results doc; Grok audit before anything goes public | |
| 26 Oct | Devpost write-up, FEEDBACK.md, "what changed since 26 August" | |
| 27–29 Oct | Video under 3 minutes; submit 29 Oct (deadline 30 Oct 22:30 IST) | |

## Open decisions

- [P004](decisions/P004-target-reset.md): **rejected 18 Oct.** "Beats" is allowed, scoped to the test, run and scorer. The goal is the top 2 blind (Cursor 66.7 / Augment 73.2 on Cal.com). **18 Oct chat update:** the blind result (3rd of 6, ahead of Greptile in 5 of 6 cells) makes the honest framing *stronger*, not weaker; still no "beats" headline because of the spread.
- Decided 2026-10-16 (Sunny): (a) first, as 059 pool-verify-v1. Next after it: 060, augmented precision for every reviewer plus a second judge.
- Decided 2026-10-16 (Sunny): the Action switches to pool-verify-v1; the 060 second judge is Qwen3-235B-A22B-Instruct-2507; 060 covers the second judge plus augmented precision.
- Decided 2026-10-16 (Sunny): **stop tuning, freeze now**; demo on **both** TraceHound itself (dogfood) and a seeded-bug demo repo.
- Cal.com run: pool-verify-v1 primary. Chat recommends declaring diff-v1 and pool-raw as secondary rows in advance; they come free from the same pipeline.
- "What changed since 26 August": TraceHound's git history starts 2026-09-30, and decisions 001–031 first appear that same day, so they were likely carried over from earlier work. Is the earlier history (FixFlow) somewhere public? Sunny to answer.
- Own-money cap for non-Nebius comparison arms, if any are ever run.

## Before starting relay, every time

- The PR relay waits on is merged.
- `git -C ~/Projects/TraceHound status --porcelain` prints nothing.
- The caps match the brief.
- Never rerun a brief whose PR is already open.

## v2 architecture (19 Oct, proposed, Sunny to approve)

See [project/architecture-v2.md](project/architecture-v2.md): S1 pre-filter → S2 contract map → S3 generators → ~~S4 vote~~ (lost in 064) → S5 evidence verifier → S6 post-gate → S7 deliver → S8 learn. Freeze-2 is on 24 Oct, followed by **one** blind Cal.com run of v2. **No standalone Cal.com run for 064** (nothing was adopted). Ultrareview #2 moves to the int-01 branch. Codex and UI end 22 Oct; Claude B takes integration and UI.

## Open PRs (checked by chat 19 Oct 03:40; all three merge cleanly with main and with each other; merged together locally, analyzer 724 tests pass and typecheck is clean)

- **#46** 064 vote, head `d76eeb3`, draft, CI green. **Loss, recorded as one. Merge it** (mark it ready, which runs the dogfood review for a few cents; use a merge commit).
  - Every table recomputes: comments = TP+FP+dup+low and TP+FN = 74 in all 18 rows.
  - The means recompute: v1 F 42.3 / P 40.5; x1 F 51.3 / P 50.9.
  - Commit order: plan `57f0b91` at 21:09 IST, then `vote.ts` at 21:13 IST, both before any run finished.
  - Accepted: two test files under `packages/analyzer/test/`.
  - **Not accepted:** the builder edited `tracehound-ops/ops/lanes.json` (cap 0 → 10). 065's brief now forbids any edit to tracehound-ops. Sunny: check what lanes.json says.
- **#45** codex-08 gate (+08b): **merged by Sunny 19 Oct 13:46 → main `b98f753`**. Chat re-ran the full suite on main afterwards: action 153, analyzer 778 (+7 skipped), viewer 186; typecheck clean; the `gate` usage line has `--base`, and `contracts` prints its usage. Main CI run 39221589986 was in progress at 13:55.
- **#41** cc-01 audit: **merged** (in main `9a81505`).
- **#48** 065 evidence verifier + gate, head `d2e5a6b`, draft, CI green. **A loss, recorded as one. Merge it** (mark it ready, merge commit). Checked by chat 19 Oct 16:20: all 40 table rows recompute; means v1 baseline F 42.3 / P 40.5, A 41.6 / 40.3, B 38.3 / 37.2; x1 baseline 51.3, A 50.4, B 45.7. Freeze `9bfb3eb` came before the driver and the first call, and the diff after it is empty. Spend $2.651 → **ledger $25.82**. The evidence checks dropped TPs at the pool's base rate (13 of 59), and contracts made 0 findings on dev (mostly non-TS repos).
- **#42** ReviewBench spike (draft, `scripts/reviewbench/` + doc): **leave as a draft** until Sunny decides on ReviewBench (it needs Anthropic API credit: ~$3 for the practice set, ~$7 for a 60-PR run). If it's a no by 22 Oct, close it unmerged; R7 keeps the findings.
- **#47** cc-03 contracts, head `d469e28`, draft. Paths are in scope; 141 tests; 10 breaks caught, 0 findings on the 10 controls; 0 findings on 9 real commits; excalidraw takes 53–65 s against a 120 s budget. **Merge** (mark it ready). Known gaps are written up: package types are `any` without node_modules, and route mount prefixes are ignored.

**Blindness (from 19 Oct):** no TraceHound lane (relay, Codex, Claude B, cloud builders, Grok) and not the chat may open `linea-seeded`, `recall-seeded` or `tracehound-seeds` until scoring starts on 25 Oct. Every brief from now on says so.

## Credit (20 Oct 18:50)
**Claude cloud: $52** (Sunny). The handoff's ~$85 was two days stale; most of the drop is the rb-01 session (#50), not this chat. A big builder job costs roughly $20–30 of cloud credit. From now on: rb-02 and int-01 go to **relay** (Pro plan), and the cloud is only for rb-01b. The chat keeps PR checks lean.

## HOLD #49 (20 Oct 19:05): codex-09b first
Checked by chat on `30a3d44`. Paths are in scope; analyzer learn tests 47/47 and action tests 174/174 pass; `learn: 'off'` is unchanged; the permission check is right.

**Defect** (it came from the brief): code spans are dropped, so identifiers vanish. The probes:
- a rejected null-deref on `user` suppressed a null-deref on `order` in the same file;
- it also suppressed a template-alike comment in another `.ts` file.

Fix: [codex-09b](prompts/2026-10-20/codex-09b-learn-fixes.md), in the same Codex task. **Must land by 22 Oct.**

Grok bots: [grok-09](prompts/2026-10-20/grok-09-bots.md) has three jobs:
- A: ReviewBench portal questions. Includes who pays the judge, and whether we can re-register a digest for candidate 6.
- B: X and LinkedIn drafts, with the Verifier checking numbers against results.md.
- C: a daily digest.

## 20 Oct 20:50: 3.5 not callable; relay stuck; #52 nearly done
- **Nemotron 3.5 Super VL: listed but HTTP 404 on chat** (067 builder: 35/35 calls failed, $0). `GET /v1/models` lists it next to 3 Super, which works. 067 stopped correctly after run 7's first calls (branch `build/067-dev-3.5`, `cf5b70b`). **Sunny:** try it in the Token Factory playground, copy its exact API id and base URL from the model page's code sample, and ask Nebius support with the FEEDBACK entry. P008 amendment 4: 3.5 candidates are dropped if it's still uncallable when the test runs start. Smoke run 3 (3.5) will go red until it's fixed.
- **Relay is stuck by design, not a bug:** it polls CI for `cf5b70b`, but CI runs only on `main` pushes and PRs, and the branch has no PR. **Ctrl-C the relay.** 067 resumes once 3.5 answers.
- **#52 (Bunny 066): cleared to merge** at `e91a268` (066c fixed the three leftovers; tests green locally). Nit: `BENCHMARK_REVISION` pins `e54ee63`, whose tables are identical; re-pin it in any later viewer job.
- **Devpost ([R14](research/R14-devpost-checklist.md)):** **no competitor names or logos in the video** (trademark rule); the demo must work without a judge's key; MIT LICENSE exists.
- **Terms ([R13](research/R13-tool-terms.md)):** Cursor allows publishing benchmarks with a full replication kit; Greptile §1.4(a) bars sharing "any results" without OK (ask Greptile); CodeRabbit is Sunny's call (kept).

## Lanes and jobs (20 Oct 20:20)
| Lane | Job | Status |
|---|---|---|
| **Sunny** (you, by hand) | close #42; rebuild the ReviewBench image on main → 4 smoke runs; register in the portal; decide on the judge API key (~$20) or the P008 amendment 3 fallback; send Verifier B | now |
| **Bunny** | [bunny-066b](prompts/2026-10-20/bunny-066b.md): finish 066 (viewer sync, CodeRabbit wording fix) | same session |
| **Codex** | [codex-10 int-01](prompts/2026-10-20/codex-10-int-01-contracts.md): contracts in the Action, default on, fail-soft. Gate and evidence verifier **not** wired (065 lost) | new task, by 22 Oct |
| **Claude cloud** | Ultrareview #2 on int-01's PR once it opens. Otherwise reserve | waits |
| **Grok bots** | [grok-10](prompts/2026-10-20/grok-10-bots.md): D (benchmark clauses in Greptile/Bugbot/CodeRabbit terms), E (Devpost submission checklist) | now |
| **Sunny's Claude Code (relay)** | [067](prompts/2026-10-20/067-dev-nemotron-3.5.md): Nemotron 3.5 on dev, 3 runs, cap $8, pre-registered adoption rule (≥ +3.0 F, P not lower, x1 not lower). Moved earlier: it's dev-only, so it can't bias ReviewBench | **now** |
| **Bunny, next** | [bunny-02](prompts/2026-10-20/bunny-02-judges-readme.md): judges' README with number placeholders | after 066b's PR |

**Post plan (Sunny, 20 Oct):** one LinkedIn and X post once the numbers are in, about 26 Oct: ReviewBench final, the 3.5 dev verdict, the seeded test and the v2 blind Cal.com run. Sunny will probably fund the Anthropic API key for the test-run judge.

**Chat decision (20 Oct):** v2 in the Action = pool-verify-v1 + **contracts** (+ `learn`, off by default). The gate and the evidence verifier stay out (065). This is what freeze-2 (24 Oct) and the seeded test (25 Oct) run.

## 20 Oct 19:50: #49 and #51 cleared, #50 merged
- **#50, #49 and #51 merged** (main includes `review-missed` and learn `dee2bed`). #50 → `8193651`. Next: rebuild the image on main, then 3 smoke runs (pool-raw 3 Super, pool-verify-v1 3 Super, pool-raw 3.5) **plus a 4th, pool-raw-missed on 3 Super, after #51 merges**.
- **#49 cleared:** head `9b21e70`, chat re-probed. A different variable in the same file and template-alike comments in other files are now **kept**. Learn tests 60, action 179. Merge it (mark ready), then int-01.
- **#51 cleared** (rb-02, candidate 6): head `99f35d0`. The chat recomputed the prompt sha256 (`a5a2115f…53bc`, now in P008). Frozen prompt files and `packages/action` are unchanged; missed tests 10, adapter 46. The prompt is generic. rb-02 ran in the cloud, not relay. Merge it.
- **Judge cost ([R12](research/R12-reviewbench-portal.md)):** test runs are judged at **our** cost (Sonnet 5, about $3 each, so about $18 for 6). The final run is paid by ReviewBench. **Sunny decides:** fund an Anthropic API key (all 6 candidates), or take the fallback in P008 amendment 3 (candidates 2, 5 and 6 only, about $9). Register in the portal **now**; approval takes up to a business day and a new digest later needs no re-approval.
- **Test runs may start** once the 4 smoke runs are green and the new digest is registered.
- **Builders report via the brain:** Bunny (and Sunny's Claude Code) write their final report to `inbox/` only and push it. The chat files it, checks the PR, and updates NOW/prs. Builders never edit anything else ([inbox/README](inbox/README.md)).
- **#42** (ReviewBench spike, draft): close it unmerged now that #50 has merged; R7 keeps the findings.
- Names: **Sunny** = account A's local Claude Code; **Bunny** = account B (`claudeb`).

## Lanes (20 Oct 19:30)
- **Relay:** rb-02, started 19:22 by Sunny. Its step 0 needs #50 merged.
- **Claude B:** a second Pro account, set up per [claude-b-setup](prompts/2026-10-20/claude-b-setup.md), in `~/Projects/TraceHound-claude2` with `CLAUDE_CONFIG_DIR=~/.claude-b` and no `.env`. First job: **066** (moved from relay, Sonnet). Then int-01.
- **Codex:** codex-09b (ends 22 Oct).
- **Cloud:** reserve only ($52).
- **Grok bots:** grok-09.

## #50 CLEARED (20 Oct 19:15): rb-01b checked, merge it
Head `6bb935c`, 5 new commits, all in scope, no `packages/` changes, adapter tests 38/38 (chat re-ran them). The smoke test now fails on any non-ok status and on zero total findings. Image `sha256:95cc69d2…`, anonymous pull works. The builder corrected the brief: the cap is crossed by the verifier's 4 concurrent calls, not by 2 reviewer calls. **The 3.5 id is still unconfirmed:** smoke run 3 (pool-raw, 3.5) is the confirmation, because a wrong id now turns the run red.

### Earlier: HOLD #50 (20 Oct 18:55)
Checked by chat on head `1d06203`:
- 27/27 adapter tests pass on Node 22.
- The never-fail wrapper, the 40-finding cap and the contract schema check are fine.
- `golden/` is banned in the docs.
- The image workflow run #1 is green. **Sunny:** read its summary for the digest and whether the package is public.

Gaps:
1. **No 3.5 price.** The $5/$15 placeholder × 32k `max_tokens` breaks the $1 per-process cap, so every 3.5 call is refused and the run silently scores 0.
2. **The smoke test can't fail.** The wrapper always exits 0.
3. The smoke test has no model input.

Fix: [rb-01b](prompts/2026-10-20/rb-01b-nemotron-3.5.md), in the same cloud session as #50.

**P008 amendment 2 (decided by the chat, Sunny delegated):** candidate 6 is `pool-raw-missed`, built by [rb-02](prompts/2026-10-20/rb-02-missed-issue-pass.md) (new cloud session, after #50 merges). Cutoff 23 Oct 12:00 IST.
- **No ReviewBench test run of any candidate starts until the chat has recorded rb-02's prompt sha256 in P008.**
- If rb-02 misses the cutoff, drop candidate 6 and start the runs.

## grok-08 (20 Oct 18:20)
- Filed → [R11](research/R11-grok08.md). Kill test 5/5 real, 3 overstated. **No P008 amendment**: its top 3 are `pool-raw-x2` (already candidate 3) or new prompts/code. Adds one rb-01b check: 3.5 smoke must report `finish_reason` (reasoning shares `max_tokens`). **ChatGPT pass (18:35):** kill test 5/5 (LAURA misattributed to callers/callees; Qodo extended numbers only in charts). New: **missed-issue pass** (CR-Bench: +5.75 recall on GPT-5.2, closed models, iterative). Chat recommends **no** candidate 6; **Sunny decides before any test run**. rb-01b also checks `learn` off in the image.

## ReviewBench (19 Oct 18:20): the main lever toward "above Greptile and Cursor"
- 219 PRs, run by GitHub + Microsoft; grounded F1 is driven by recall; Greptile 27.2, Cursor 17.3, Copilot 40.1 ([R7](research/R7-cc02-reviewbench.md), [R9](research/R9-reviewbench-methodology.txt)).
- **Pre-registered: [P008](decisions/P008-reviewbench-preregistration.md).** Never open `golden/`. Choose between pool-verify-v1, pool-raw and pool-raw-x2 from the 25-PR test runs only; then one final run.
- Prompt `rb-01-reviewbench-image.md` (cloud session): adapter, Dockerfile, ghcr build, smoke workflow. Then Sunny: portal registration → approval → 3 test runs → final.
- **Nemotron 3.5 Super is on Token Factory at the same price** as 3 Super (SWE-bench Verified 71.1 vs 60.5); see [R10](research/R10-smarter-reviewer.md). It was added to the ReviewBench candidates (P008 amendment 1) and to rb-01's support list. Fine-tuning isn't possible: Token Factory post-training has no Nemotron.
- Needs a Nebius top-up of about $25. Sunny has Nebius **sandbox** access (beta); it's not needed for ReviewBench, but it's a candidate for "checked fixes" later.

## Prompt queue (19 Oct 03:45)

| Lane | File (prompts/2026-10-19/) | Where | Status |
|---|---|---|---|
| Codex | `codex-08b-gate-fixes.md` | the **same** Codex task as #45 | ready |
| ~~Relay~~ | ~~065~~ → **#48, a loss** | | done |
| Relay | `066-results-docs.md` (R6 fixes, all-50 with a dev 5-run-mean row, levers table, README headline; **$0**) | `~/Projects/tracehound-ops` → TraceHound | after #48 merges; Sunny sets `nebiusCapUsd` = 0 |
| Codex | `codex-09-learning.md` (S8: 👎 / resolved-unchanged / reply → suppress; maintainers only; `learn` input default off; conventions reader, not wired) | ChatGPT Codex, `~/Projects/TraceHound-codex`, new task | **ready, must land by 22 Oct** |
| (old) Relay | `065-evidence-gate.md` | Run 1 stopped at step 0 at 13:00 IST ($0). Brief patched. **#45 is merged now, so it's ready to rerun**: re-copy the brief from the brain, then start. lanes.json = 5 (confirmed by relay) |
| ~~Grok~~ | ~~grok-07~~ → [R8](research/R8-competitors-learning.md): links recovered and 5 of 5 checked. **Sunny keeps CodeRabbit** (seeded test + public numbers) despite §4.2(iv) of its terms. Seeded-test tools: TraceHound, CodeRabbit, Greptile, Bugbot (Bugbot ≈ $1–1.5 per run) | | done 19 Oct |
| Cloud | ~~cc-04 + cc-04b~~ **done 17:40**: `tracehound-seeds` `a4a6c83`. Scorer has 13/13 offline checks; `open-prs.sh` triggers CodeRabbit and opens one PR per 450 s (≈3 h 40 m for 30). **Pre-registration (published 19 Oct 17:50 IST):** `sha256(key.json) = abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1`, `sha256(SCORING.md) = ebac16b3961512720c8c85463ce0b504aa1cab2fbb3b18dbafa026f3acaa5c1d`. 25 Oct: install the TraceHound Action (freeze-2 pin), CodeRabbit, Greptile and Bugbot on both mirrors, then Sunny runs `open-prs.sh` | | done |
| Codex | codex-09 learning (S8) | — | after codex-08b |
| Claude B | int-01 integration (`th-v2` in the Action) | `~/Projects/TraceHound-claude2` | 22 Oct |

## Prompt queue (18 Oct 21:45). Every lane busy, no path overlaps

| Lane | File | Where | Waits on |
|---|---|---|---|
| Relay | `064-vote.md` (voting across runs, dev only, cap $10) | `~/Projects/tracehound-ops` → TraceHound | `nebiusCapUsd` = 10 |
| ~~codex-07~~ → **#43** | | |
| ~~ui-02~~ → **#44** | | |
| ~~cc-01~~ → **#41** draft, [R6](research/R6-cc01-audit.md): numbers all correct; 5 wording overclaims in results.md; gaps within noise except CodeRabbit | | |
| ~~cc-02~~ → **#42** draft, [R7](research/R7-cc02-reviewbench.md): probably feasible; 25-PR practice set ≈ $4; judge needs Anthropic API money | | |
| Grok | ~~grok-05~~ → [R4](research/R4-precision-techniques.md) (done 18 Oct; Cursor's early Bugbot = 8-pass vote + validator) | | |
| Grok | ~~grok-06~~ → [R5](research/R5-hackathon-and-nebius-levers.md) (done 18 Oct; Coding track favours plan → test → iterate agents; weights unknown) | | |

Path guard: relay owns `scripts/bench/`, `docs/bench/` except the cc-01/cc-02 files, and `docs/decisions.md`; Codex owns `packages/action/`, `.github/`, `docs/action/`; UI owns `viewer/`, `docs/ui/`; cc-02 owns `scripts/reviewbench/`.

## Credits
- Nebius console $39.93 (18 Oct 21:00).
- **Ultrareview: 3 free runs** (Claude cloud bug-finding agent, about 15–25 min each). Spend them only on branches before merge, highest risk first:
  1. codex-07 branch (adds `contents: write` and pushes the orphan branch; touches other people's repos);
  2. 064 branch (voting and scoring code that the headline number depends on);
  3. reserve: the final pre-submission branch (27–28 Oct), or ui-02 if it renders untrusted comment text.
  Never on main, and never on docs-only PRs.
- Claude cloud: **$89 of $100 left, expires 5 Nov 13:29 IST**. It's cloud-session credit only (screenshot 18 Oct), **not API credit**, so it can't pay the ReviewBench judge (Claude Sonnet 5 API). Judge cost is about $3 for the 25-PR practice set and about $7 per 60-PR run; that needs a separate Anthropic API top-up (Sunny's call).
- (Superseded line follows.) Claude cloud $96, expires 5 Nov. Spend it on audits, spikes, judges' README drafts and TariffLens setup after 29 Oct.

## Honest claim status (18 Oct, revised after cc-01)
- **Public line:** "on 10 blind PRs, statistically tied with Greptile and Cursor, ahead of CodeRabbit; 4th of 6 across all 50 PRs; every loss published." Don't say "3rd of 6" as a ranking.
- Blind Cal.com: 3rd of 6 (10 PRs, n = 23).
- All 50 PRs: **4th of 6** (41.2 vs Greptile 49.5, CodeRabbit 40.0).
- Sunny does not want claims that draw "how did you beat CodeRabbit?" questions, so the post wording is "3rd of 6 blind, 4th of 6 across all 50, losses published".
- The top-2 push (064) is judged on all 50 PRs, not on Cal.com alone.
