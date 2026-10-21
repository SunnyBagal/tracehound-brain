# bunny-03: per-model base URL (Nemotron 3.5 lives on us-north1)

Tool: **Bunny**, a **new `claudeb` session** in `~/Projects/TraceHound-claude2`. Paste everything below the line. No `.env`; make no Nebius calls.

---

Nebius serves the two models we use from different regions (Sunny's curl, 20 Oct 22:37):

| Model | us-central1 (`https://api.tokenfactory.us-central1.nebius.com/v1`) | us-north1 (`https://api.tokenfactory.us-north1.nebius.com/v1`) |
|---|---|---|
| `nvidia/nemotron-3-super-120b-a12b` (and Nano, Qwen) | works | `model_not_found` |
| `nvidia/Nemotron-3.5-Super-VL` | 404 "does not exist", although `/v1/models` lists it | works |

Our client has one base URL for every call (`DEFAULT_BASE_URL`, or `NEBIUS_BASE_URL`). So no single process can call 3.5 and 3 Super together. Job 067 needs both (3.5 reviewers, with Super and Qwen judges), and so do ReviewBench candidates 4–6. Fix it in the client, the one path every model call takes.

## Step 0 (if any check fails, STOP and report)
- `git fetch origin`; `git status --porcelain` is empty. Branch `fix/model-base-url` from `origin/main`, which must be `7de5dbd` or later.
- In a plain shell, the `GIT_*` env check prints nothing. Then `pnpm test` and `pnpm typecheck` are green.
- Read:
  - `packages/analyzer/src/llm/client.ts`, `cache.ts`, `ledger.ts` and `setup.ts`;
  - `scripts/reviewbench/entry.ts` (around line 311);
  - `docs/bench/reviewbench.md`;
  - `.github/workflows/reviewbench-smoke.yml`.
- **Don't open** `SunnyBagal/linea-seeded`, `recall-seeded`, `tracehound-seeds`, or anything under ReviewBench's `golden/`.

## What to build (one commit per item, each with tests, no network)
1. **Region routing in the client.**
   - In `client.ts`, add an exported `MODEL_BASE_URLS` table with one entry: `"nvidia/Nemotron-3.5-Super-VL": "https://api.tokenfactory.us-north1.nebius.com/v1"`.
   - The URL for a request is chosen per request, from the request's `model`:
     - if the configured base URL's host is a Nebius Token Factory host (`api.tokenfactory.<region>.nebius.com`) and the model is in the table, use the table's URL;
     - otherwise use the configured base URL unchanged. Any other host (tests, a proxy, `models.example.invalid`) is never rewritten.
   - Model ids match exactly (case-sensitive), as Nebius matches them.
   - Tests:
     - 3.5 with the default base goes to us-north1;
     - 3 Super with the default base stays on us-central1;
     - 3.5 with `NEBIUS_BASE_URL` = us-central1 goes to us-north1;
     - 3.5 with a non-Nebius base URL is unchanged;
     - an unknown model is unchanged.

     Use the existing fake `fetch` and assert the URL it received.
2. **Cache and ledger.**
   - Check whether the response-cache key and the ledger record include the base URL. If the cache key does, it must use the **resolved** URL. A test shows two calls for different models in one client hit different URLs and don't share a cache entry.
   - Change no cached response format. If old cache entries would stop matching for 3 Super, **stop and report** instead.
3. **ReviewBench adapter.**
   - `entry.ts` prints `base_url=…`. Make it print the resolved URL for the configured model, so a 3.5 run logs us-north1.
   - Update `docs/bench/reviewbench.md`:
     - 3.5 is on us-north1 and the client routes it;
     - the portal registration's egress list already has `api.tokenfactory.us-north1.nebius.com` (onboarding PR review-bench/ReviewBench#92).
   - The smoke workflow keeps `MODEL_BASE_URL` at us-central1; routing happens in the client. Add one comment line saying so.
   - Adapter tests: `pnpm exec vitest run --config scripts/reviewbench/vitest.config.ts` and `pnpm exec tsc -p scripts/reviewbench`.
4. **FEEDBACK.md.** Append one dated entry below the existing content. Write only these facts and invent nothing:
   - `GET /v1/models` on us-central1 lists `nvidia/Nemotron-3.5-Super-VL`, but chat completions there return HTTP 404 "does not exist" (35/35 calls in 067, 20 Oct).
   - The same id answers on us-north1 (one call, 20 Oct).
   - 3 Super is `model_not_found` on us-north1.
   - The console's code sample shows us-north1.
   - What would have helped: `/v1/models` per region listing only callable models, or a region field.

   Opinions are labelled as opinions. No key.

## Rules
- Touch only:
  - `packages/analyzer/src/llm/client.ts`, plus `cache.ts` only if item 2 needs it;
  - `packages/analyzer/test/` (the client tests);
  - `scripts/reviewbench/entry.ts` and its tests;
  - `docs/bench/reviewbench.md`;
  - `.github/workflows/reviewbench-smoke.yml` (comment only);
  - `FEEDBACK.md`.
- No prompt changes. The frozen prompt sha256 pins must still pass.
- No new dependencies. No Nebius or model calls (`test/setup.ts` blocks the network).
- Run the gates as their own commands, read the result, then commit separately.
- Open a **draft** PR titled `fix: route Nemotron 3.5 to us-north1 (per-model base URL)`. Wait for CI; make one fix attempt if it's red. No force-push, no merge.
- Brain: write your final report to `inbox/2026-10-21-bunny-03.md` following `inbox/README.md`, and push only that file.

End with: the PR number, head sha, one line per commit, test counts (workspace and adapter), what the cache key contains and whether it changed, and anything you couldn't do.
