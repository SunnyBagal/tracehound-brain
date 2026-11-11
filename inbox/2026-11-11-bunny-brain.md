# bunny · brain backfill (2026-11-11)

Brief from artu1: `run/briefs/bunny-brain.md` (private test-set repo). The repo sources were read at TraceHound main `cd5525a`. $0, no model call, no network write. **Nothing in the brain is committed or pushed**: codex reviews the working-tree diff, then artu1 pushes. The untracked `inbox/2026-10-29-codex-20.md` is not mine and was not touched.

**Note on the inbox rule.** This inbox's README says builders write only here and never edit NOW, timeline, lessons or `decisions/`. This brief told me to write those files, so I did, and I'm flagging it here. Every note is marked `judged_by: bunny (backfill, from the repo record)` so the chat can re-judge it.

## Files added (22)
- Decision notes (14): `decisions/D062-calcom-result.md`, `D063-results-page.md`, `D064-vote.md`, `D065-evidence-gate.md`, `D066-results-wording.md`, `D067-nemotron-35.md`, `D068-partial-reviews.md`, `D069-v2-calcom.md`, `D070-recall-v3.md`, `D071-ultra-dev.md`, `D072-freeze-2-1.md`, `D073-recovery-ab.md`, `D074-hard-chunks.md`, `D075-verify-a-vs-e.md`.
- Process note (1): `decisions/P010-herdr-team.md`.
- Topic notes (7):
  - `research/R17-multi-agent-review-2026-11-07.md`
  - `project/empty-replies.md`
  - `project/300s-transport-cutoff.md`
  - `benchmark/cache-survivorship.md`
  - `benchmark/track-b-testset.md`
  - `ops/ledger-vs-billing.md`
  - `ops/local-runner.md`

## Files changed (7)
- `decisions/_index.md`:
  - rows 062–063 added (they were missing);
  - rows 064–065 now link their notes;
  - rows 066–075 added;
  - P010 row added.
- `decisions/digest.md`: entries 059–075 appended in the existing format, with a dated comment saying they were added by hand.
- `timeline.md`:
  - dated entries 2026-10-25 → 2026-11-11;
  - a new "Phase 5" heading;
  - the stale hackathon "Ahead" line replaced with the current sequence.
- `README.md`: 5 map rows added (chunk failures, cache rule, Track B, local runner, Herdr team) and 2 extended (spend → ledger vs billing; research → R17).
- `research/README.md`: R17 row.
- `lessons.md`: 3 lessons (2026-11-08 cache replay, 2026-11-09 billing lag, 2026-11-10 firewall slips).
- `NOW.md`:
  - the stale top (frontmatter, 30 Oct handoff pointer, 23 Oct schedule block) replaced with an 11 Nov state section;
  - the older dated entries are kept below a divider.

## Verdicts given (outcome / call)
| note | outcome / call |
|---|---|
| D062 | tie / sound |
| D063 | n/a / **flawed** (overclaims later fixed by 066) |
| D064 | loss / sound |
| D065 | loss / sound |
| D066 | n/a / sound |
| D067 | win / sound |
| D068 | n/a / sound |
| D069 | tie / sound |
| D070 | loss / **flawed** (per the existing 2026-10-24 lesson) |
| D071 | n/a / sound |
| D072 | win / sound |
| D073 | tie / **flawed** (replay baseline, per 073a) |
| D074 | win / sound (superseded) |
| D075 | loss / sound |
| P010 | pending / too-early |

## Checks
- **Relative links:** 0 broken in the 29 changed files. The one `[[…]]` hit is pre-existing `[[TBD-SEEDED]]` text in NOW.md's older entries.
- **Privacy grep over all added text** found 0 hits for:
  - case ids (`case-`, `ho-`);
  - 5-digit PR numbers;
  - the split seed;
  - key prefixes;
  - email;
  - dev-PR paths (`keycloak/`, `sentry/`, …);
  - held-out paths.
- **GitHub anchors:** computed with GitHub's heading-slug rule (as the existing index rows are), not fetched.

## Left out, and why
- **Track B:** case ids, case and fix PR numbers, shas, labels, bug descriptions, the replacement list, and the split seed value. Public vault; counts and method only.
- **Firewall slips:** which agent, and which candidates. They are described in general terms only.
- **075:** the dev PR/chunk named in the failed judge call. It isn't needed for the verdict.
- **Research report (R17):** model price comparisons and the credit balance quoted in REPORT §1/§5 (only budget.md's public figures are used), and each agent's terminal address.
- **D061:** still says "Verdict pending (after 062)". I did not change it (not in the brief), so the chat should update it from D062.

## Correction (2026-11-11)
The report above is kept verbatim. It describes the vault as it was when I wrote it. After that, artu1 changed two things on Sunny's ruling:
- `inbox/README.md` now lets agents edit the curated notes they are assigned. So "Note on the inbox rule" above no longer describes the current rule.
- D061's verdict was completed (outcome and call). So the "D061 still says pending" line above is superseded.

"29 changed files" in "Checks" means my original scope: 22 added notes plus 7 changed files, not counting this report. codex's review covered 33 files after artu1's additions.

Fix round 1 (after codex's NO-GO, `run/out/codex.brain-review.md`):
- D070 and the digest's 070 entry: the floors are precision floors, and both arms fail them.
- The seeded totals in the timeline are labelled unverified.
- `ops/local-runner.md`: the credential and paid-run wording is corrected.
- `benchmark/track-b-testset.md`: the clean-case definition is corrected.
- `NOW.md` and the Track B note: the unclear-case ruling and the standing rules are added.
