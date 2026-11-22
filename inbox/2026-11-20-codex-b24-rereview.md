GO aafb18f20dc06453a8db8921b87bbd6edeff72d3

Re-reviewed `33b7958..aafb18f` (two commits, three files, +119/−25) in fresh detached worktree `/private/tmp/codex-b24-rereview-aafb18f`, carrying forward the complete seven-file review in `codex.b24-review.md`. Current main: `bf0893170fa7475f4f0876a7749e64f17b110766`. All four prior findings are closed; no new findings. $0, no model call. H1 duties retain priority. This GO is for W1 preparation at this exact sha; merge still waits until H2's commit is on main, as 076aa requires.

| prior finding | evidence at aafb18f | result |
|---|---|---|
| 1, exact p | `scripts/bench/w1-fill.ts:185@aafb18f` (`P_FIELD`), `format` line 194 | Primary, pooled and secondary p fields use the scorer's `Number(p.toPrecision(10))` form. Independent invented-input replay now prints `0.00390625` for both primary and pooled p, rather than pooled `0.00`. Null p prints `–` in all three namespaces. Undefined inputs remain unresolved. Regression coverage is at `packages/analyzer/test/w1-fill.test.ts:84@aafb18f`. |
| 2, chronology | `docs/bench/ts-heldout.md:361@aafb18f`, `:402@aafb18f` | Both §9 and limit 4 use the narrower 076u item-4 wording required by 076aa item 6. The superseded alternative/choose-one marker is removed. Limit 4 retains the earlier-benchmark origin, dev-use/history and secondary-pooling caveats. |
| 3, E=0 cells | `docs/bench/ts-heldout.md:189@aafb18f` (three rows) | Both exposed cells per row now have explicit E=0/E>0 branches. Independent replay gives `0 of 0 (no exposed bug cases)` in both cells. Tests cover all three tools, E=0, E>0, and missing E retaining visible unresolved markers for `--check`. |
| 4, limit content | `docs/bench/ts-heldout.md:469@aafb18f`, `:477@aafb18f` | Limit 13 restores the boundary against inferring what the file names could reveal, and the no-file-name statement. Limit 14 restores committed-count provenance, explicitly naming H2 §7 and ts-dev. Limit 6 is unchanged from the prior substantive-content check; its later count/vendor-source verification remains W1 work. |

076aa singular addendum and wording scope

- Checked the main addendum at `bf08931` and both fix commits, including the appended bunny report. `singular` (`scripts/bench/w1-fill.ts:210@aafb18f`) is applied only after a placeholder formats to `1` (`fill`, line 293). Its explicit noun/verb changes cover bug/bug case/case, pull request, first trigger, comment/item/miss/minute, `1 of the … was`, and `1 needed its`. Other filled values retain the template words.
- The invented whole-page regression at `packages/analyzer/test/w1-fill.test.ts:190@aafb18f` covers the §3b exposure sentence, below-floor sentence, trigger retry, sheet size, guess counts and comments clause. It checks that the plural forms do not remain after a filled count of 1. The preceding test preserves `7 bugs` and `1 bugsy`.
- Independently replayed the original finding reproducer against the new worktree: outcome C still has the same counts, n, p and caution sentence, with `1 bug` as the grammar correction. The format change restores p precision; it does not change the tested value or branch choice.
- Reviewed every documentation hunk: only the required E=0 encoding, ruled chronology correction, restored limit clauses and associated explanatory comment changed. No other §3/§4 wording changed in the delta. Outcome selection, alpha/floor logic, summary resolver, Wilson scale, costs and frozen scoring/policy inputs are unchanged. README, results.md and both viewer files are byte-unchanged from the previously reviewed head.
- Existing unresolved writer inputs still remain visible, with `--check` failing until they are filled outside comments. Manual prediction verdicts, vendor fact verification, committed result counts, source pin and Sunny's final reading are still W1 checks. The operator-item commitments in the previous review stand, including fixed K/19 sensitivities and H1 total chunks beside failed chunks; none was estimated from held-out inputs in this review.

Validation

Node v26.13.1; plain-shell GIT_* guard printed nothing before every gate. Fresh worktree uses existing dependency links, no install and no `.env`/ledger link. Test packages ran sequentially with two Vitest workers, unchanged deadlines and no cache. Tests ran outside the sandbox because the same transport/process tests had established sandbox permission failures in the first review; this rerun had no failures.

| gate | observed result |
|---|---|
| typecheck | PASS, all 3 packages |
| analyzer | PASS, 1080 passed / 8 skipped; 61 passed files / 6 skipped (67); includes 17 filler tests |
| action | PASS, 423 tests / 22 files |
| viewer | PASS, 225 tests / 30 files, including report-table equality and rendered-table coverage with placeholders |
| viewer Webpack build + static export verification | PASS, 8/8 static pages, `/benchmark` included |
| `git diff --check main...aafb18f` | PASS |
| merge preview against main bf08931 | PASS, no conflicts; tree `489f18d26fcda8705054f2625f91427629f7833f` |
| final tracked worktree status | clean |

The default Turbopack build was not rerun: the first review established its inability to follow this isolated worktree's dependency symlink outside its inferred root. As before, validation used `pnpm exec next build --webpack` and `node scripts/snapshots.mjs verify-export`, without source/config edits. No default-bundler success is claimed. Logs and independent reproduction: `/private/tmp/codex-b24-rereview-evidence/`.

PR: none. Reviewed head: `aafb18f20dc06453a8db8921b87bbd6edeff72d3`. Codex changed only this report, its claim/done, sender-owned handoff and an uncommitted raw brain report. No source fix, dependency addition, provider call, held-out/window-directory read for this review, commit, merge, push or publication. Any later branch commit voids this GO; only this sha is eligible after H2's commit is on main. H1 watch/snapshot duties continue unchanged.
