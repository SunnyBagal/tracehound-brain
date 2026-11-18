GO 698d9021ccd19f91e3bff623a92bccc4f095689f

One pass of `docs/bench/ts-dev.md` on branch `b20-ts-dev` at `698d9021ccd19f91e3bff623a92bccc4f095689f`. Parent `a2c157bab762b4664ee3965871139a41c9856cb4`. Commit 2026-11-17 09:31:06 +0530. One file, 251 insertions. Reviewed with `git show` of that sha. The worktree HEAD was a later commit. Spend $0. No merge and no push. Held-out was not opened. Dev only. `run/out/bunny.ts-dev.md` was read and not edited. This file does not make a second §2 reading. Counts only. No case ids.

## Copied rows

The catch table on 31, location hits, comment means and medians, clean-case comments, the v1 pairs on 31, and the v1–x1 agreement on the seven 31-case rows match `run/out/grok.e1-check.md` (every row MATCH, both judges). The page's stop/go section reports decision 076t's reading of that recount: **RETHINK**. The four booleans are the recount's:

| condition | figures on the page | holds |
|---|---|---|
| A0 catches ≥ CodeRabbit − 3 | 12 ≥ 16 | no |
| comments per case ≤ 2 × CodeRabbit | 1.075 ≤ 6.150 | yes |
| CodeRabbit leads by 6 or more | 7 ≥ 6 | yes |
| CodeRabbit completed fewer than 30 of 40 | 40 of 40 | no |

Beside the rule, as on the page and in the recount: x1 d = 8; on the 21 unexposed, v1 d = 5 and x1 d = 4; Greptile 18 and 18. 076k item 4, as 076t states it, does not gate the tag.

## Numbers computed for this page

Recomputed from the E1 per-case analysis, the exposure list (10 of 31 bug cases, 21 unexposed, sha256 prefix `5374b210`), and the official review files. Wilson intervals use the z and formula in `scripts/testset/kit.ts`. Sign tests use that file's two-sided binomial tail. All of the following match the printed figures.

- Wilson 95% on the 21, both judges, all seven rows.
- Sonnet on its 8 bug cases (5 unexposed) and A0 on the same sets: Sonnet v1 2 and 1, x1 4 and 3; A0 v1 3 and 3, x1 4 and 4. The printed intervals match.
- Max comments per case: A0 5, CodeRabbit 10, Greptile 10, A1 8, A2 7, Haiku 4 and 4, Sonnet 4.
- Reviews with 0 comments: A0 official 19 of 40, CodeRabbit 9, Greptile 7, A1 14, A2 14, Haiku 18 and 24, Sonnet 4.
- Sonnet v1–x1 under Qwen3-30B: both / v1-only / x1-only / neither = 2 / 0 / 2 / 4, agreement 75.0%, kappa 0.500.
- Sign tests vs A0. v1: Haiku draw 1 discordant 4 and 6, p 0.753906, printed 0.754; draw 2 discordant 4 and 3, p 1.000; Sonnet discordant 2 and 1, p 1.000. x1 on the 30B details: Haiku draw 1 discordant 3 and 5, p 0.726562, printed 0.727; draw 2 discordant 4 and 1, p 0.375; Sonnet discordant 1 and 1, p 1.000.
- Pair cells for x1 on 31 and for both judges on the 21 match the page.
- Haiku draw to draw: v1 draw-1-only 4, draw-2-only 1; x1 draw-1-only 5, draw-2-only 0.

Official-attempt A0 cost prints as $0.0567 / $0.0445 / $0.2232, total $2.26957. Every-attempt runner total $2.80987. The every-attempt mean / median / max $0.0702 / $0.0511 / $0.4373 are the summary figures already matched in the recount.

## Commits and the other sections

- Official A0 records: 2 at `346fc7a7f9ff007e33d606acfe2f511bc532e398`, 38 at `997504328ea5f85926814fd43085cca721449c91`. The diff is 16 documentation files. No path under `packages/` or `scripts/`.
- Haiku commits on 40 of 40 each: `9bedd9d95d646c5a2cab95f184c8223da543a5a1`, `580ede77666cbd42445bfdbf6362250a07e8f32d`. Sonnet on 8 of 8: `76757e9213a85e9e41180993cda46f84f2bc9c62`.
- Protocol revision 10 committed 07:52:05Z and merged 13:18:39Z on 14 Nov. The E1 job file contains 14:57:06Z. R1's judge window, job spend $2.82003, runner $2.80987, and the two chunk failures match `run/out/bunny.r1.md`.
- `86bb9eb79d9b79e7af5f879a19c08654ba978d1d` is an ancestor of `742574dd943f3aaf980f7dd4b505a62cda2ac881`. L1 $0.14367 is 076j's $0.0287747 plus $0.09553 and $0.01937. 12 E1 processes plus 7 under 076j.
- P1 window end `2026-11-16T20:49:31Z`. Status files: 40 completed each.
- L3: 13 of 42 and 36 of 42, 4 bug cases. Sheet: 95 items, printed 61.3. Blind-check file: 8 headings; the word "marked" does not occur.

Notes, not conflicts: two p-values are three-decimal rounding; the R1 "no code change" is documentation only; the judging cell names the 16 Nov E1 checkout beside the 14 Nov window, and the next bullet says Claude v1 is reused from 076j.

No product tests. No PR. No merge. Decision 078 was not reviewed. Source: `tracehound-testset/run/out/grok.ts-dev.md`. Uncommitted. No push.
