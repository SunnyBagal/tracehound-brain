# cc-04 output (Claude cloud on tracehound-seeds, pasted by Sunny 19 Oct 15:02). Checked by chat: see NOW. Hashes superseded by cc-04b.

Repos attached and cloned. Now checking that each mirror's `main` matches its source (SunnyBagal/Linea, SunnyBagal/Recall).

Both `main` SHAs match their sources. Now exploring structure and installing deps.

Linea typecheck on `main` passes (4/4 packages). Linea has no test suite; checking `scripts/smoke.sh`, then installing Recall.

Recall's frontend typecheck and backend tests (38/38) pass, but the backend typecheck already has 5 pre-existing errors on main, so I'll use that as a baseline and have verify.ts flag only new errors rather than penalize seeds for existing ones. Next I'll read through the code to plan the seeds.

Still reading code to plan the 30 changes; moving to Recall now.

Now the Recall frontend.

I have a working picture of both codebases. Checking a few more call sites before I fix the seed plan.

Plan settled: 15 branches per repo (X-typed 3/3, X-keyed 3/3, X-semantic 2/2, D 3/3, C 4/4). Building the Linea branches now.

Building Linea branches (3 of 15 done).

Linea: 7 of 15 branches done. Continuing with the socket-hook change.

Linea: 11 of 15 done; now the 4 clean controls.

All 15 Linea branches are built (local only, not pushed). Starting Recall.

I noticed some of my commit messages and code changes described or fixed behavior that already existed, which violates your rules. I'm now reworking those branches to correct this.

Now redoing the three Linea branches (ws reply helper, ops route, selection change).

Reworked. Continuing with Recall (4 of 15 done).

Recall: 8 of 15 done; now the in-diff defects.

That commit message described an existing behaviour in Recall (duplicate usernames), which your rules forbid. Replacing that branch with a different check-then-act case.

Recall: 11 of 15 done; now the 4 clean controls.

All 30 branches are built locally (nothing pushed yet). Now pinning exact accept line numbers at each branch head.

Line numbers collected. Running a quick scan of every branch for the banned words before writing the private repo files.

One hit ("hit-test" in a commit message). Rewording it.

Now writing the private repo files (`key.json`, `prs/`, `SCORING.md`, `verify.ts`, `score.ts`, `open-prs.sh`, `README.md`).

`key.json` and the 30 PR files are generated. Next: `verify.ts`. First checking that the typecheck commands leave the clones' working trees clean.

`verify.ts` gives 30/30 PASS on the local branches. Quick negative check that it actually catches mistakes:

The negative check fails each corrupted row as expected. Now `score.ts`, with offline fixtures.

Restricting the rule (b) candidate list to review bodies and summaries only, to match the pre-registered wording.

`score.ts` works on fixtures (8/8 checks pass). Now `open-prs.sh` (written, not run), `SCORING.md` and `README.md`.

Writing `README.md`, `package.json` and `.gitignore`.

All files are in place. Pushing the 30 branches to the mirrors (branches only: no PRs, `main` untouched).

Pushed. Confirming both mirrors' `main` is unchanged, then running the full `verify.ts` against the pushed remote branches.

30/30 PASS against the pushed branches. Committing and pushing `tracehound-seeds`.

The test set is built and pushed: 30 branches on the mirrors (15 per repo, no PRs opened), plus the answer key and tools on `tracehound-seeds` `main`. `verify.ts` passes all 30 rows against the pushed branches.

**Step 0 baseline.** Both mirrors' `main` match their sources (Linea `4f075a5`, Recall `20ac149`), and neither `main` was changed.
- **Linea:** the typecheck has 0 errors. The repo has no automated test suite; `scripts/smoke.sh` needs live servers and a database, so tests show `n/a`.
- **Recall:** the frontend typecheck is clean and the backend tests pass (38/38). The backend typecheck already fails on `main`, with 5 errors (`index.ts` ×3, `services/embeddings.ts`, `worker.ts`). I didn't stop for that: I recorded them as the baseline. In `verify.ts`, "typecheck passes" means no new errors compared with `main`, matched by file, error code and message so a shifted line doesn't count as new.

**verify.ts**
```
| id  | repo   | class      | branch                               | commit | size | edited | types     | tests | accept | nohint | result |
|-----|--------|------------|--------------------------------------|--------|------|--------|-----------|-------|--------|--------|--------|
| s01 | linea  | X-semantic | feat/socket-reconnect-option         | ok     | 15   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s02 | linea  | X-typed    | refactor/submit-state-naming         | ok     | 8    | ok     | ok(2 new) | n/a   | ok     | ok     | PASS   |
| s03 | recall | X-keyed    | chore/queue-redis-prefix             | ok     | 5    | ok     | ok        | ok    | ok     | ok     | PASS   |
| s04 | linea  | C          | feat/hide-grid-in-history            | ok     | 14   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s05 | recall | C          | feat/escape-clears-search            | ok     | 9    | ok     | ok        | ok    | ok     | ok     | PASS   |
| s06 | recall | C          | refactor/chat-context-fields         | ok     | 35   | ok     | ok        | ok    | ok     | ok     | PASS   |
| s07 | recall | X-typed    | fix/ai-summary-parse-failure         | ok     | 17   | ok     | ok(8 new) | -     | ok     | ok     | PASS   |
| s08 | linea  | C          | refactor/move-socket-hook            | ok     | 6    | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s09 | recall | D          | feat/search-result-limit             | ok     | 9    | ok     | ok        | -     | ok     | ok     | PASS   |
| s10 | recall | D          | feat/dedupe-saved-links              | ok     | 11   | ok     | ok        | -     | ok     | ok     | PASS   |
| s11 | linea  | X-typed    | fix/ops-load-auth-expiry             | ok     | 7    | ok     | ok(2 new) | n/a   | ok     | ok     | PASS   |
| s12 | linea  | X-keyed    | feat/rest-room-operations-route      | ok     | 13   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s13 | recall | X-semantic | refactor/search-query-first          | ok     | 7    | ok     | ok        | -     | ok     | ok     | PASS   |
| s14 | recall | C          | feat/metadata-timeout-option         | ok     | 12   | ok     | ok        | ok    | ok     | ok     | PASS   |
| s15 | recall | C          | refactor/chat-sources-event          | ok     | 7    | ok     | ok        | ok    | ok     | ok     | PASS   |
| s16 | linea  | C          | refactor/chat-broadcast-event        | ok     | 7    | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s17 | recall | X-semantic | fix/embedding-errors-surface         | ok     | 42   | ok     | ok        | -     | ok     | ok     | PASS   |
| s18 | recall | X-keyed    | chore/namespace-auth-storage         | ok     | 14   | ok     | ok        | ok    | ok     | ok     | PASS   |
| s19 | linea  | X-keyed    | chore/namespace-auth-env             | ok     | 11   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s20 | linea  | C          | refactor/rename-state-at-seq         | ok     | 12   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s21 | linea  | X-typed    | feat/server-message-schemas          | ok     | 43   | ok     | ok(1 new) | n/a   | ok     | ok     | PASS   |
| s22 | linea  | D          | refactor/pencil-segment-helpers      | ok     | 24   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s23 | linea  | D          | feat/delete-room-endpoint            | ok     | 25   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s24 | recall | X-typed    | refactor/card-variant-prop           | ok     | 11   | ok     | ok(1 new) | -     | ok     | ok     | PASS   |
| s25 | recall | X-typed    | feat/abortable-text-extraction       | ok     | 17   | ok     | ok(2 new) | -     | ok     | ok     | PASS   |
| s26 | linea  | X-semantic | refactor/zoom-limits-percent         | ok     | 17   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s27 | linea  | X-keyed    | refactor/ws-ack-event-names          | ok     | 25   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s28 | recall | D          | feat/ai-answer-formatting            | ok     | 50   | ok     | ok        | -     | ok     | ok     | PASS   |
| s29 | linea  | D          | fix/clear-selection-on-remote-update | ok     | 14   | ok     | ok        | n/a   | ok     | ok     | PASS   |
| s30 | recall | X-keyed    | refactor/content-search-route        | ok     | 8    | ok     | ok        | ok    | ok     | ok     | PASS   |
30/30 pass
```
A `-` in the tests column means the class doesn't require tests. I also corrupted four entries in a copy of the key and the PR text, and `verify.ts` failed each one. `score.ts` passes its 8 offline checks against the fixtures and made no API calls.

**Class counts per repo**

| | X-typed | X-keyed | X-semantic | D | C | Total |
|---|---|---|---|---|---|---|
| Linea | 3 | 3 | 2 | 3 | 4 | 15 |
| Recall | 3 | 3 | 2 | 3 | 4 | 15 |

**Hashes**
```
abbde0097f11358f91b55b10886e257731f840d262d1fd6df12d55b275391bc1  key.json
339c8b53e9c6e92615e5cda1fb684ad63c725a6f552b820163a7172b8c3ea159  SCORING.md
```

**`tracehound-seeds` head:** `e98720d6369e65ad86cc8f3952e5bc583ca03fc2`

Things to know before 25 Oct:
- **Rule (a) can't fire for the X classes.** GitHub only allows inline comments on files in the PR's diff, and every X accept path is an unedited file. So all 16 X seeds can only be caught through your manual rule (b). I copied `SCORING.md` exactly and only noted this in the README. If you want to change the rule, do it before you publish the hashes.
- **Tool logins.** I set no default login for TraceHound, because finding it would have meant opening the repo. `score.ts` refuses to run until you set `TRACEHOUND_LOGINS`. The Greptile (`greptile-apps[bot]`) and Bugbot (`cursor[bot]`) defaults should be checked on a real PR.
- **PR bodies.** `open-prs.sh` drops the title line from `prs/<id>.md` before passing the body, so the title isn't repeated inside the PR. It also checks that `gh` is logged in as SunnyBagal and skips branches that already have a PR. I never ran it, not even with `--dry-run`.
- **Commit authorship.** Commits on the mirrors are authored by this session's git identity (Claude). Their messages end with `Co-Authored-By: Claude` and `Claude-Session` lines. None of them contains a banned word. If that might tip off reviewers, the branches would need re-authoring, which means a force-push I didn't do.
- **Branches I redid before pushing.** A few early commits described or fixed behaviour that already exists in the repos. I rewrote them before pushing, and swapped one D seed (a duplicate-username check) for `feat/dedupe-saved-links` with a different check-then-act race.