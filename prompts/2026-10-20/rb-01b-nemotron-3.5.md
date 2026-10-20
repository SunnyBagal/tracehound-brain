# rb-01b: Nemotron 3.5 support and a smoke test that can fail

**Where:** Claude cloud (claude.ai/code), the **same session that built #50** (`rb/image-v1`). If that session is gone, start a new one on TraceHound, check out `rb/image-v1` and read `docs/bench/reviewbench.md` first.
**Before pasting:** Sunny confirms the 3.5 model id in the Token Factory console. If it isn't exactly `nvidia/Nemotron-3.5-Super-VL`, replace it everywhere below.
**Spend:** $0. There's no Nebius key in this session. The real smoke runs happen after merge, by Sunny.

Paste everything below the line.

---

You're adding commits to PR #50 (`rb/image-v1`, head `1d06203`), TraceHound's ReviewBench image. It was built before Nemotron 3.5 Super was added to the candidates. The chat's check found three gaps; fix them on the same branch.

## Blindness (applies to every TraceHound job)
- Never open, read, grep or copy anything under ReviewBench's `golden/`, its judge or classifier prompts, or any test-set PR's expected issues.
- Never open the `linea-seeded`, `recall-seeded` or `tracehound-seeds` repositories.
- Run no ReviewBench judge.

## Allowed paths
- `config/prices.json`
- `scripts/reviewbench/**`
- `.github/workflows/reviewbench-image.yml`
- `.github/workflows/reviewbench-smoke.yml`
- `docs/bench/reviewbench.md`

**Nothing under `packages/`.** If a fix needs a `packages/` change, STOP and report.

## Step 0
- `git log -1 --format=%h` prints `1d06203` (or a later commit of yours on this branch).
- `git status --porcelain` is empty.

## 1. Price entry (this is a blocker today)
Add this to `config/prices.json`:

```
"nvidia/Nemotron-3.5-Super-VL": { "inputPer1M": 0.30, "outputPer1M": 0.90, "placeholder": false }
```

Extend `_readme` with the source: the Nebius blog of 16 Oct 2026, "three new open-weight models from US companies", and the Token Factory model page.

**Why it's a blocker:** without the entry, 3.5 is priced at the $5 / $15 placeholder. One review call reserves about 15k input tokens and 32,000 `max_tokens`, roughly $0.55 at that price. Two concurrent calls cross the default per-process cap of `TRACEHOUND_BUDGET_RUN_USD=1`. The call is refused, the never-fail wrapper writes zero findings, and a whole test run scores 0 with exit 0.

Add a test under `scripts/reviewbench/` that:
- loads the real `config/prices.json`;
- asserts both the 3 Super and 3.5 ids resolve to a non-estimated price;
- asserts that two concurrent reservations at 60,000 input characters and 32,000 `max_tokens` fit under the default run cap. Use the analyzer's own `Budget` and price lookup; import them, don't copy them.

## 2. Reasoning setting with 3.5
- The stages send `chat_template_kwargs: { enable_thinking: true }` (`REASONING_ON`, `packages/analyzer/src/review/review.ts:15`). NVIDIA's 3.5 Super VL model card documents the same flag. Confirm by reading the code. Don't change it.
- In `docs/bench/reviewbench.md`, add a note: on 3.5, reasoning tokens share `max_tokens` (32,000 per call). A truncated reply fails its chunk, so pool-raw fails the PR and the status line says so.

## 3. The smoke test must be able to fail
Today, `reviewbench-smoke.yml` fails only on a non-zero exit, and `entry.ts` always exits 0 on reviewer errors. So a run where every call was refused looks green. Change it:
- Add a `model` input of type `choice`: `nvidia/nemotron-3-super-120b-a12b` (the default) and `nvidia/Nemotron-3.5-Super-VL`. Pass it as `RB_CONFIG_MODEL`.
- **Fail the job** if any PR's adapter status line isn't `status=ok`, or if every PR returned 0 findings.
- The summary table gains `model`, `cost_usd` and `findings` per PR. Some already exist; keep them.
- `ChunkRecord` doesn't store `finish_reason` (the chat checked). A truncated reply shows up as `CHUNK_FAILED` with a `failure` string, and `readReview` already puts the first failure in the status line. Add a test that a stage output with `chunkFailed: 1` and `failure: "invalid reply"` gives a status line containing that text. Don't touch `packages/`.

## 4. Learning stays out of the image
- PR #49 (codex-09, learning) isn't merged yet. Add a static test that fails if `scripts/reviewbench/*.ts` imports anything from `packages/action/src/data.ts`, `fix.ts`, or any file whose name contains `learn`.
- Add a line to `docs/bench/reviewbench.md`: the image never reads or writes a data branch, and runs with no learned memory.

## 5. Docs
- `docs/bench/reviewbench.md` lists the pre-registered candidates from the brain's P008, 1–5:
  1. pool-verify-v1 on 3 Super;
  2. pool-raw on 3 Super;
  3. pool-raw-x2 on 3 Super;
  4. pool-verify-v1 on 3.5;
  5. pool-raw on 3.5.

  Add one line: "Candidate 6 (`pool-raw-missed`) arrives in rb-02." Link P008: `https://github.com/SunnyBagal/tracehound-brain/blob/main/decisions/P008-reviewbench-preregistration.md`.
- Add the smoke procedure after merge (see the end of this prompt).

## 6. Finish
- Run the adapter tests and typecheck, as `reviewbench-image.yml` does. Every test must pass.
- Commit in small steps on `rb/image-v1`, push, and leave #50 as a draft.
- Update #50's description with:
  - what changed;
  - the test count before and after;
  - the cap arithmetic from step 1;
  - the new image digest from the build workflow's step summary;
  - whether the summary says the package is public.

Report back the same in under 25 lines.

### After merge (for Sunny, not this session)
The smoke workflow only appears once it's on `main`. Run `ReviewBench image` on main and take its digest. Then run `ReviewBench smoke` three times:
1. pool-raw, 3 Super;
2. pool-verify-v1, 3 Super;
3. pool-raw, 3.5.

Spend is a few cents each. All three must be green.
