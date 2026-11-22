REHEARSAL PASS — dev inputs only, $0, no provider/network call

CURRENT SUNDAY ARTIFACT UPDATE — machine-safety ruling d57085c, 2026-11-20T05:55:36.571522+00:00: disk guards added and tested. Use run/out/codex.machine-safety.ack and the current artifact-manifest SHA256 92fdbff6aa5ef45bf850c84053fd0323dac6e4250948d65985e4d00e5fceda7f; original47ff9804… fingerprints below are historical rehearsal evidence. Registered verifier/exposure/probe bytes and all model/data rules are unchanged.

Prepared 20 Nov IST (execution 19 Nov UTC) from frozen tag `review-freeze-3`, head `25151b1d6dbb9d7d8feec92a6b7a76db14107e1f`. H1 duties remained first priority; the last operator confirmation was 19:00:03Z, and the six-hour watch remains armed. No held-out file, label, window directory, scheduler state/pins or fork was opened for this rehearsal. No real H2 step started. Main/tag ledgers stayed unchanged, all 22 source/config/document hashes listed in ts-freeze match, and the tag's tracked tree is clean.

Sunday's ready-to-paste list: `/private/tmp/codex-h2-sunday-commands.md`. It has six staged Bash blocks, with explicit stop gates; it is not an unattended worker and was not executed on held-out inputs. Before using it, compare `/private/tmp/codex-h2-artifacts.sha256`'s SHA256 to **`47ff9804c8e68469bd2b33b5d921396f32c7fc9ad8488f7b72fd77b171be7548`**, then run its `shasum -c`. The list checks those component bytes again. Losing/changing an artifact is a preflight failure, not permission to substitute code during the run.

What had already run, and what was rehearsed now

| step | prior evidence; exact tag-worktree command? | this rehearsal |
|---|---|---|
| H1 §6 collect + hash | P1's saved collection/manifest/resume files and `bunny.p1.md`; F1's frozen-input hashing in `codex.ho-freeze.md` / `codex.f1-heldout.md`. Those are not a completed held-out snapshot from this tag worktree. The 72-hour snapshot has not happened. | Ran the tag collector CLI six times on P1's cached raw dev objects through a local-only `--gh-bin` shim, with original trigger resumes/bounds. Exported review files are byte-identical to saved dev exports; statuses agree for all40 cases per external tool. Rehearsed selected-attempt merge, SHA256 generation and verification in scratch. No real `gh` or network call. |
| assemble | F1 B1/B2 tag tests and real dev integrations; E1's real replay/selection before the final tag. These are useful evidence, not a claim E1 ran from this later tag worktree. | Selected 40 official A0 records (6 reruns), 40 Haiku draw1 and 40 draw2 (0 reruns each), then ran the tag's `replay-arms.ts` on all three. Exit0, no A0 replay mismatch. Rehearsed separate collection groups and checked merged status coverage/duplicate refusal. |
| verifier and L0 guard | **Yes for the verifier**: F1 §4.2 ran the unchanged second Node block of `tag-worktree.md`; H1 repeatedly used it, recorded in `codex.f1-heldout.md` and `codex.ho-run.md`. No real H2 L0 exists yet. | Re-ran that exact verifier block from th-h1: samePhysicalLedger/envSame/trackedClean true, hardStop61.0898, matching ledger totals. The ledger helper printed a number and the rehearsal `L0+.40` fits. Wrote only `REHEARSAL L0` in scratch; did not set or reset Sunday's H2 baseline. |
| frozen judging order | E1's actual chain was not this H2 row order (and reused some existing details). No full H2 paid rehearsal is claimed. | Statically checked all14 proposed invocations: A0 v1/x1, CodeRabbit v1/x1, Greptile v1/x1, Haiku1 v1/x1, Haiku2 v1/x1, A1 v1/x1, A2 v1/x1. They exactly follow H2 §2. No judge command ran; existing dev details supplied all pre-check/sheet inputs. |
| private pre-check | E1's `l1.summary.json`, location output and `bunny.e1.md`; F1 tag kit fixtures. No exact final held-out-input command could have run before the snapshot. | Ran tag `location-hit.ts` and `score-testset.ts` with saved dev v1/x1 details, raw A0 attempts for costs, statuses and the common exposure mask. Also ran the production primary-policy wiring (`policy.076c.json`, with `--side dev`) into a separate explicitly rehearsal output. Both pre-check runs exit0; no inference from those rehearsal tests is used. |
| seeded, stripped sheet | E1's first sheet-size build had no strip and163 tool-name hits, so it is **not** the production blinding check. Its later stripped artifact plus F1's B2 tag tests are prior evidence, not today's exact seed/input run. | Ran tag `handcheck-sheet.ts build` with seed20261118, frozen strip and policy: **95 dev items**, tool-identifier hits0. Repeated it: sheet, decisions, mapping and hashes all byte-identical. The production-policy sheet also has95 items/0 hits. Ran `--x1-fallback` with no x1 details: **262 dev items**, 0 hits. No decisions were filled and `resolve` never ran. |

Review-time exposure was also rehearsed, including source ancestry queries. The operator-only helper reads saved branch logs, resume pushes, frozen case/base/head identities and raw completion/counting times; it checks branch snapshots precede trigger commands, rejects unknown case branches/tip mismatches, uses the latest counted/completion-object time, and adds pushes present by that time. Git ancestry is local with lazy fetch, replacement objects and network protocols disabled. Its common union is **10 of31 dev bugs**, exactly the saved independent `grok.dev-exposure.ids.json` mask; each external tool is10/31. The final helper run checked62 completed-review flags with366 cached ancestry probes: unknown branches0, mismatched tips0, missing snapshots0, missing trigger snapshots0. This exercises the mechanism, not Sunday's actual exposure; Grok must independently derive Sunday's flags before judging.

Measured local wall times

| operation | seconds |
|---|---:|
| frozen verifier / ledger helper | 0.315 / 0.137 |
| official selection, all3 dev rows | 0.062 |
| six cached collector invocations, first direct pass | 10.854 total (1.151–2.365 each) |
| reusable collection helper, fresh six-collection pass | 10.203 summed collector time |
| merge / replay A0 / replay Haiku1 / replay Haiku2 | 0.016 / 0.113 / 0.110 / 0.106 |
| location / first pre-check | 0.083 / 0.116 |
| production primary-policy pre-check | 0.149 |
| normal sheet / identical repeat / fallback sheet | 0.095 / 0.097 / 0.096 |
| production primary-policy sheet | 0.098 |
| final review-time exposure recount | 3.872 |
| original scratch hash verify / blank-sheet hash | 0.075 / 0.023 |
| reusable window-manifest hash + verify (391 scratch files) | 0.140 |

These measure local parsing, cached API replay and disk/git work. They do not measure Sunday's live GitHub latency, model service/retry latency, independent Grok turnaround or the human sitting. E1's saved timestamps provide historical paid timing, not a guarantee. The ~06:00Z sheet target remains a target; no window end, cap, judge order or missing-data rule may be changed to meet it. Local no-model work took seconds; live collection, provider checks/judging and independent checks determine Sunday's critical path. Start the actual collection at04:30:06Z, no earlier and no later than05:30:06Z.

Command-list contents and boundaries

- ~04:15:06Z: Node v26.13.1 via Vite Plus PATH; source and private-artifact digest checks, unchanged tracked tag and lockfile, global ledger/env verifier, caffeinate/AC/lid and scheduler end/status. This is future execution, not a claim those live checks ran in this dev rehearsal.
- At04:30:06Z: record the first collection start and H2 claim without replacing an existing claim. The read-only collector uses separate triggered/untriggered/never-opened groups, explicit fixed window bounds, per-attempt output directories and verified selection receipts. Read-failed retries preserve every attempt. Hash actual H1 outputs, **both nested Haiku draws**, scheduler/resumes and every collection attempt. Complete H1 §7's final counts/deviation/hash report from those real inputs.
- Assemble originals/reruns with the **076y** sources `$HO/th-haiku-1/draw-1` and `$HO/th-haiku-2/draw-2`; replay each selected row. Merge only selected complete collections, never the runbook's unrestricted glob over attempts. Count unlisted raw top-level authors/Bot logins. Derive review-time exposure locally and wait for Grok's independent union and per-review flags; compare canonical copies and preserve/hash originals.
- Existing H2 step0 checks: one minimal ledgered request per frozen judge, then original financial L0 once. The supplied availability helper has date/arming/model guards; it was **syntax-checked only**, never invoked. Existing request receipts are reused/validated, not overwritten by a repeated block. A v1 check failure stops; x1 unavailability uses registered fallback(d).
- Judge one process at a time in the fixed14-entry order. Each invocation repeats the global verifier, reads the first original H2 L0, enforces the one shared `$0.40` envelope within hard stop61.0898, refuses existing detail/log names and checks JUDGE_FAILED after it finishes. There is no automatic retry or automatic advancement after a failure. One allowed retry uses `.b.json`; the later actual detail is official. Separate flags distinguish a fallback sheet from established service unavailability, so one bad verdict does not silently cancel later available x1 work.
- Use latest-detail pointers for private pre-check/sheet. Normal mode requires primary x1 evidence; fallback(d) includes **every counted comment on every reviewed primary bug case**, including already-scored x1 cases. Secondary rows stay judge-only. Seed20261118 and frozen strip remain unchanged. Check0 identifiers; hash blank sheet, decisions, mapping and recipe.
- The exact notification point follows those checks/hashes: `hc send t9 'H2 stripped sheet ready for Sunny; blank sheet and mapping hashed. Please arrange the sitting. No counts or outcomes in this notice.'` No count is put in that line. Sunny receives only `sheet.md` and blank `decisions.json`, never mapping, judge tables or logs. At a split-sitting stop, hash partial decisions; no unblinding until his last item and final filled-file hash confirmation. H2 §6's subsequent score/hash/commit/lift sequence and prior K/19/chunk/count-report commitments stand.

Fallbacks and who decides

| condition | action / owner |
|---|---|
| x1 genuinely unavailable at step0 or fails during judging | Codex applies the already-registered fallback(d), records when/why and actual affected coverage. Every primary reviewed bug comment goes on the sheet. No third model, no hand-check of secondary rows and no manufactured x1 detail. A transient failed process can get its one allowed retry if the original cap has room. |
| judge request/process failure or cap refusal | Codex inspects the private receipt. One rerun of that process into a new `.b` detail if the original `$0.40` has room; later real file is official. Preserve earlier evidence. v1 step0 failure, missing/refused v1 evidence after allowed recovery or an uncovered condition stops for Sunny's ruling. No rebaseline, reserve transfer or new spend approval follows from this rehearsal. |
| snapshot preflight/hash/binding/scheduler check fails | Stop the dependent operation. Codex/grok diagnose under H1 §5; Sunny rules on uncovered/missing/late snapshot conditions. No shifted cutoff or invented collection, no scheduler/pin repair in this command list. Grok is the H2 stand-in if codex is blocked/unavailable as the H2 brief specifies. |
| unlisted Bot, unknown case branch, missing branch evidence or codex/Grok exposure disagreement | No judging. Codex/grok investigate; Sunny receives required counts/login facts and rules where the runbook requires. Do not silently choose another exposure mask or edit frozen tools. |
| a tool cue survives frozen stripping | No hand editing of strip.json. Sunny decides whether to accept the known cue or void the sheet. |

Artifacts and evidence

| Sunday artifact | SHA256 |
|---|---|
| `/private/tmp/codex-h2-sunday-commands.md` | `ce640a11b91092655bca1ea3d127170183972fd47bcb0d13cfd74a2838c9b8d0` |
| `/private/tmp/codex-h2-prepare.py` (`collect`, `assemble`, `hash-window`; no paid CLI) | `47529e8810c9b0d6894668f2d2d28a2aa85ed9546f2329b2716639000a178fbe` |
| `/private/tmp/codex-h2-exposure.mjs` (local derivation only) | `bc95dbd0181c37029a5190c72f9801d597f63ee52b1415c2ce57122336432720` |
| `/private/tmp/codex-h2-live-probe.mjs` (**not run**, future paid step0 only) | `9cc09e1bf1e79a428b33b93babefb20d06e7d4959861f5bf513d783c71301710` |
| `/private/tmp/codex-h2-rehearsal-20261120-r2/verifier.mjs` (unchanged tag documentation block) | `019f567f7a74ba3dc2b80034b131410e6e66543ae2e7e0c34ed860c7a95f2fd6` |

Private dev evidence: `/private/tmp/codex-h2-rehearsal-20261120-r2/` and `/private/tmp/codex-h2-prepare-dev/`. The final rehearsal manifest covers836 evidence files; SHA256 **`a8a682311769becf54a2a3d6c71bb9f9b98823a51eda9598ac533854428de4f1`**. It is dev rehearsal evidence, not `hashes.window.txt` for the live window. All six Bash blocks and four inline Python/two inline JavaScript blocks parse; the availability helper parses; the proposed14-entry judge list exactly matches H2's frozen order. Parsing did not execute those Sunday blocks.

Diagnostics retained: the first wrapper stopped after a successful verifier because it looked for `hardStopUSD` at the top level instead of under `main`. Corrected only the scratch wrapper, retained its initial directory, and restarted into a fresh `-r2` directory; no provider call or ledger change. The saved P1 plan has38 positions because two dev cases were already served in S1. The exposure rehearsal therefore uses a scratch40-case population map from the dev manifest, with both S1 and P1 branch logs; no source case list was edited and no case was excluded. The independent mask still agrees exactly.

PR: none. Source head: `25151b1d6dbb9d7d8feec92a6b7a76db14107e1f`. Product tests/typecheck/build: not rerun (no product change); operational rehearsal results/timings are above. Files written: this report/claim/done, sender-owned owner handoff, raw uncommitted brain note and dev scratch artifacts/helpers. No main/tag source, frozen/pinned input, ledger, dependency, scheduler, fork, held-out artifact, actual judge detail or hand-check decision was changed. Actual H1 §6 snapshot, real H2 model checks/judging, actual exposure/Grok comparison, Sunny's sheet/sittings, unblinding, final commit and firewall lift remain future frozen obligations.
