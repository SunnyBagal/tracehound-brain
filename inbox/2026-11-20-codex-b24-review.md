NO-GO 33b7958d391738a14b518da4e04fb0ab97526d25

Reviewed all seven files (+1055) in `main...33b7958`, including b23 and both b24 commits, in the fresh detached worktree `/private/tmp/codex-b24-review-33b7958`. Comparison main: `c84e7086f83f29e05caf677168aab116f8a2ad87`. Cost $0; no provider call. No held-out directory/window, case note, fork, review object, or score input was opened for this review. H1 operator duties retain priority. No merge, source edit, commit, push or PR.

Findings (citations at the reviewed sha)

1. **P1 — Preserve exact p-values, including the pooled table.** `scripts/bench/w1-fill.ts:187@33b7958`, `format`. Only `pairs[...].p` receives a p formatter, and that uses three significant figures; `pooled:coderabbit.p`/`pooled:greptile.p` fall through to two decimals at line 191. Reproduction with invented inputs: p=0.00390625 becomes primary `0.00391` and pooled **`0.00`**. The scorer prints `0.00390625` (`scripts/testset/score-testset.ts`, `main`, line 331, ten significant figures). The pooled table labels this an exact two-sided p, and protocol §4 requires exact p. Use the scorer's precision for every p field, retain a dash for counts-only/ineligible results, and test the pooled small-p path. This can publish a nonzero probability as zero without leaving a placeholder.

2. **P2 — Apply the chronology correction already settled by 076aa.** `docs/bench/ts-heldout.md:360@33b7958` and `:405@33b7958`. §9 still presents the superseded “before any result” sentence as an unresolved choice, and limit 4 states it unconditionally. Decision 076aa item 6 explicitly requires the narrower 076u wording in both places because the old claim overstates the chronology. Replace the choice and limit-4 opening with “pool-verify-v1 was fixed before any catch count of it on the dev cases existed; contracts off was chosen by a model-free dev audit”, retaining the other required limit content. No result is needed to make this correction.

3. **P2 — Encode the required zero-exposure table branch.** `docs/bench/ts-heldout.md:189@33b7958`. W1 §2 item 3b and H2 §7 require the exposed cells to say `0 of 0 (no exposed bug cases)` when E=0. The table always prints numeric placeholders; only an HTML comment instructs a later correction. With an invented E=0 summary, it fills the row as `| TraceHound | 0 of 0 | 0 | 7 of 20 | 20 |`, and `leftovers` returns 0. Encode the conditional cells or retain a visible unresolved marker until this mandated wording is supplied; a clean fill must not silently omit it.

4. **P2 — Preserve the substantive content of the unshortened limits.** `docs/bench/ts-heldout.md:465@33b7958` and `:475@33b7958`. Comparing limits 6, 13 and 14 word by word, the directions-to-writer can be converted into page text under proposal 10, but limit 13 drops W1 §4's explicit boundary that the page claims nothing about what the disclosed names could reveal. Limit 14 also drops the statement that both zero-inline counts come from committed counts, including the H2 §7 provenance. Restore these as page statements. Limit 6 retains the substantive required clauses, the distinct fixes/bases counts, retention caveat and per-fork ref-count placeholder; its numbers/vendor facts remain subject to W1's source check, not a new inference.

Evidence for findings 1 and 3: `/private/tmp/codex-b24-evidence/reproduce.mjs` (invented data only). The reproduction also demonstrates `1 bugs`; apply 076aa item 15(a)'s singular form during the W1 fill, or implement it in the helper. No held-out value was used to choose any branch.

Checks that passed

- `Resolver.outcome` checks the registered reviewed-by-both floor before p, then uses the policy alpha and discordant direction for A/B/C. The four outcome tests include a below-floor pair with a small p and refusal of absent/unregistered pairs. The observed-gap and same-bugs riders are inside C. CodeRabbit–Greptile has counts only.
- Wilson formatting is on the scorer/ts-dev percentage scale: e.g. `23.7–56.2%`. Costs use four decimals; noninteger comment statistics use the scorer's two-decimal convention. The p exception above remains a blocker.
- Unknown fields remain listed, and undecided conditions retain markers; `--check` returns 1 for unresolved tokens outside HTML comments, as documented. HTML-comment instructions, source verification and manual prediction verdicts still require W1 review.
- The regular/fallback first-sentence variants, A/B/C/floor templates, A/B final caution, volume/cost clause, exposure definition, fixed order and README measured-configuration name are present. The narrower wording correction, E=0 cells and unshortened-limit omissions above prevent a full template GO.
- Hardcoded dev table counts/intervals, comment counts, zero-inline counts and RETHINK arithmetic agree with committed `ts-dev.md`; the Haiku disclosure retains its historical registration wording and labels its dev numbers. No held-out outcome was preselected.
- Grok's claimed fixes checked: both unqualified pairs-table condition names are gone; Wilson scale is fixed; four cohort exclusion cells are present. Thus 4 of the original 21 issues are structurally resolved. The additional bare pooled-floor sentence is now qualified per pair. The remaining 8 NO SOURCE and 9 RENAME names remain writer/operator inputs; the formerly unresolved chronology choice is now settled by 076aa and must be removed (finding 2).
- Viewer equality/render checks passed with placeholders: 225 tests, including the existing aggregate-table exact-equality and rendered-table-count checks. The new filler tests also compare filled §11 cells against the viewer source on invented inputs.
- `git diff --check` clean. Current main differs from b24's base only in STATE, LOG, FEEDBACK, budget and decisions. `git merge-tree --write-tree main 33b7958...` exited 0 with tree `581e9e5c7473567e46b64898618939eaa5f3ea39`; no conflicts at the main sha above. This is a preview, not a merge or authorization to merge before H2.

076aa operator-item verdicts

| item | verdict and obligation |
|---|---|
| 1 | ACCEPT: choose the next free result-decision number at W1; do not preassign 081 or reuse 079. |
| 2 | ACCEPT: exempt only the required “Matches were proposed…” template from the banned-word grep; existing README history is separately scoped. Other matching prose still gets reviewed. |
| 3 | ACCEPT: `codex.ho-score.md` will carry per-primary-tool completed-zero-inline, v1 catches, K/19, rejected judge matches, accepted location-only items, sheet-item count, total sitting minutes and fallback N. These are counts derived after H2, not guessed fields added to frozen scorer code. |
| 4 | ACCEPT: retain K/19 for every primary tool, no test and no headline substitution, under protocol §2/§4. |
| 5 | ACCEPT: C_x is comments per completed PR mean; D is ledger cost per attempted PR, with the console-reconciled figure beside it. |
| 7 | ACCEPT: cite 076g item 6 for the rule and 076t for the RETHINK reading. |
| 8 | ACCEPT with scope explicit: H2 §7 / `codex.ho-score.md` will report N as the distinct bug-case union with no SCORED x1 verdict for at least one completed primary-tool review, over A0/CodeRabbit/Greptile only. Preserve per-row affected counts too. Fallback still puts every counted comment on every reviewed primary bug case on the sheet, including cases with SCORED x1; secondary rows stay judge-only. |
| 9 | ACCEPT: recheck limit-6 numbers from committed counts at W1. artu1 must identify the verified vendor table; include only facts labelled D. This review grants no new vendor-fact approval. |
| 10 | CHANGE: converting writer directions into page text is appropriate, but retain every substantive clause and source attribution. Findings 3/4 identify missing requirements; limit 6 otherwise retains its required substantive content. H2 §7 / `codex.ho-score.md` will supply the completed-zero-inline and exposure counts needed by limits 6/14; H1 supplies window deviations. |
| 11 | ACCEPT: confirm actual secondary/pooled schemas at H2; rename placeholders to those schemas, never coerce/guess a missing field. A1=`pool-raw`, A2=`review-diff-v1` are already named in H2 §1. H2 §7 / `codex.ho-score.md` will identify the committed secondary/pooled outputs and their aggregate counts. |
| 12 | ACCEPT: source-pin follow-up names W1's real merged sha; it cannot name its own commit. It remains a publication check after W1, not permission to update during H1. |
| 13 | ACCEPT: codex owns/fills `ts-heldout.*`; bunny's skeleton/helper are branch inputs authorized by 076z. Owned decisions/budget/FEEDBACK/STATE/LOG remain artu1's. |
| 14 | ACCEPT: keep §11 and viewer section last; no existing anchor moves. |
| 16 | ACCEPT: H2 §7 / `codex.ho-score.md` will carry `caughtOf19` per tool/row, derived from the same official catches with the fixed unmappable case omitted: hand-checked for the primary tools, judge-only for secondary rows. Never replace K/20 or selectively relabel. |
| 17 | ACCEPT: final H1 §7 report will carry total reviewer chunks beside failed chunks, from A0's official run records with the counting basis stated. H2's report can cite that counts-only value for W1 prediction 4c. This review did not open those records. |
| 18 | ACCEPT: `ho-score:<tool>.added` means H2 §7's location-only items accepted by Sunny, not all acceptances. `codex.ho-score.md` will state that definition and counts; rename the placeholder if that is clearer. |

Items 6 and 15(a) are already ruled in 076aa; this review changes neither ruling. Findings require a new bunny commit and review on its new full sha through artu1, not edits by codex.

Validation and limits

Node v26.13.1, existing dependency links only; no install, no `.env`/ledger link. Before every test/typecheck/build invocation the required GIT_* guard printed nothing. Gates ran sequentially in plain shells with at most two Vitest workers, unchanged timeouts, no cache.

| gate | result |
|---|---|
| typecheck | PASS, all 3 packages |
| analyzer retry | PASS, 1076 passed / 8 skipped, 61 passed files / 6 skipped; includes 13 new filler tests |
| action retry | PASS, 423 passed, 22 files |
| viewer | PASS, 225 passed, 30 files |
| viewer Webpack build + export verification | PASS, 8/8 static pages, `/benchmark` present |
| default Turbopack build | FAILED: worktree `node_modules` symlink points outside inferred filesystem root; not claimed passed |

First sandboxed analyzer run: 1071 passed / 13 skipped plus a failing transport-suite setup (`listen EPERM` on loopback; hook timeout). First action run: 420 passed / 3 failed in detached-process tests under sandbox process restrictions. Retained those logs, then reran analyzer/action outside the sandbox unchanged; both passed. Viewer passed on its first run. Webpack fallback changed no tracked source/configuration. Evidence logs: `/private/tmp/codex-b24-evidence/`. Worktree tracked status remained clean after all gates.

PR: none. Reviewed head: `33b7958d391738a14b518da4e04fb0ab97526d25`. Files changed by codex: this report, claim, done marker and sender-owned owner handoff only. No source fixes. Merge remains blocked by this NO-GO and, independently, must wait until H2's commit is on main. Final W1 numeric/vendor verification, source pin and Sunny's page approval have not run; there is no publication GO.
