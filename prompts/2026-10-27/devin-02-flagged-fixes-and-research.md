# devin-02: fix what devin-01 found, behind flags that default to today's behaviour; plus a short docs research

Tool: **Devin, the same session as devin-01** (it already read the code; a follow-up message, not a new session). Paste everything below the line.

Why flags: the product every published number describes is **`review-freeze-2` (`0aeef06`)**, and the README, demo repo and seeded mirrors all pin the Action to that commit. Fixes on main don't change what judges run, but main must not silently become a different reviewer either. So: every behaviour change is **opt-in, default off**, and a test proves the default output is byte-identical to today's.

Merge rule (chat): mergeable before submission only if CI is green, the identity test is green, the chat has checked it and Bugbot has run; otherwise it merges after 30 Oct. Target: PR ready by **28 Oct 09:00 IST**.

---

Thanks, the report checked out (one correction: levers v2 did **not** show raw beating verified on 3.5; both intervals include 0, so it's an open question). Now please fix the defensible items, in one PR, under these rules.

## Rules
- Branch `devin/post-freeze-fixes` from `origin/main` (contains `5ba60a0`). **Draft PR** titled `review: post-freeze fixes behind default-off flags (devin-02)`. Never merge, never tag.
- **Default behaviour is frozen.** With no new option set, every review output, posted comment, chunk record field that exists today, cache key and ledger entry is byte-identical to main. New options are plain function/CLI options; **no new Action inputs** (don't edit `.github/actions/**`, `action.yml`, or any workflow).
- **No model calls, no API keys, $0.** Tests use stub clients only.
- **Don't touch:** any prompt text or prompt file (their sha256 is pinned in tests), `scripts/bench/agree-rule.ts` (the scorer depends on it), `scripts/reviewbench/`, `viewer/`, `snapshots/`, `docs/bench/`, `README.md`, `docs/decisions.md`, `docs/freeze.md`, `config/`, `pnpm-lock.yaml`, `.github/`. No new dependencies. Other lanes are editing `viewer/`, `docs/bench/` and `README.md` right now.
- Never open Cal.com data, ReviewBench `golden/`, `linea-seeded`, `recall-seeded`, `tracehound-seeds`.
- Gates before each commit, as separate commands: the `GIT_*` env check in `CLAUDE.md`, `pnpm test`, `pnpm typecheck`. 5 s timeouts only: rerun those files with `--testTimeout 30000`. Commit messages `area: what changed`.

## Commits, in this order
1. **Identity test first, before any change.** A test that runs `pool-verify-v1` end to end through `packages/action/src/pipeline.ts` (and the action's comment/cap step) with a stub client returning fixed replies, covering: a clean chunk, a fenced reply with prose around it, a reply cut off with `finish_reason: "length"`, an empty reply, two reviewers reporting the same file with `line: null`, a VERIFY_FAILED verdict, more findings than `max-comments`. Snapshot the full output (review body, comments, footer, chunk records). Commit it green on today's code.
2. **Observability (default on; additive only).** `ChunkRecord` gets optional `attemptsDetail: { finishReason?, reasoningTokens?, outputTokens?, contentEmpty: boolean }[]` in both reviewers. The Action's "nothing posted" log line prints stage counts (chunks ok/failed, pool, kept, posted). Neither changes any posted text; update the snapshot only for the new record fields and say so.
3. **Truncation-aware retry** — option `retryOnLength: "append" | "fresh"`, default `"append"` (today). `"fresh"`: when attempt 1 ends with `finishReason === "length"`, retry with the **original messages only** and reasoning off (keep `max_tokens`). Both `review.ts` and `review-ctx.ts`.
4. **Lenient parser** — option `parser: "strict" | "lenient"`, default `"strict"`. Lenient: strip prose around a fence, else take the last balanced `{…}` that contains `"findings"` (reviewers) or `"verdict"` (verifier). Strict path unchanged byte for byte.
5. **Line-less duplicates in the pool** — option `poolNullLines: "separate" | "merge"`, default `"separate"`, in `pipeline.ts`'s pool step only: `"merge"` treats same normalised path with both lines `null` as agreeing. **Don't change `agrees()`.**
6. **Cap order** — option `capOrder: "pool" | "agreement-first"`, default `"pool"`: `"agreement-first"` sorts `sources === "both"` before `"diff"` before `"ctx"`, stable, before `slice(0, maxComments)`.
7. **Don't cache cut-off replies** — client option `cacheTruncated: boolean`, default `true` (today). `false` skips `cache.put` when `finish_reason === "length"`. Cache keys unchanged.
8. **A $0 cache scan script**, `scripts/bench/truncation-scan.ts`: given `--cache <dir>` and `--model <id>`, reads cached response bodies and prints **counts only**: total, by `finish_reason`, and for `length`: content empty / content present but unparseable / parseable. No text printed. Tests on a fixture dir. (Note in its header: empty-content truncations are never cached by `client.ts:296`, so it undercounts them; the ledger's 32,000-token count is the other half.)
9. **`docs/post-freeze-fixes.md`**: one table — finding (from devin-01, with file:line), option name, default, what the non-default does, test name, **"not measured"**. One line on top: the product and every published number are `review-freeze-2` (`0aeef06`); these options are off by default and unmeasured.
10. Push, mark the PR ready, wait for CI; one fix attempt if red. Comment `bugbot run` on the PR.

**Not in this PR** (say so in the PR body): the job timeout (it lives in users' workflows), deduplicating the two reviewer loops (refactor risk now), the per-process spend cap, partial reviews for review-diff-v1, a reasoning budget / `response_format` (needs the research below and a paid smoke).

## Research (docs only, no API calls), in the PR body under "Research"
1. Does Nebius Token Factory support `response_format` with a JSON schema (or guided JSON) **for `nvidia/Nemotron-3.5-Super-VL` on us-north1**? Quote the doc line (under 15 words) and link it; "not stated" is a valid answer.
2. Does it document a **reasoning budget** for 3.5 Super (`reasoning_budget`, `thinking_budget`, `chat_template_kwargs` keys)? Same format. (On 23 Oct our probe found Token Factory honoured no reasoning budget for 3 Super or 3.5: `docs/bench/v3-recall-plan.md`.)
3. NVIDIA's recommended sampling for **3.5 Super** specifically (model card / NIM reference), and whether reasoning and JSON output are documented to combine.
4. For each answer, the cheapest smoke that would settle it (number of calls, which endpoint), written as a plan, not run.

## Final message
Commits (sha + one line each), the identity test's name and that it's unchanged after commits 3–8, test counts, PR number and head sha, CI state, the four research answers with links, and anything you skipped and why.
