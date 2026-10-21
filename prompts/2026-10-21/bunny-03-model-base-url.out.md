# Bunny 03 — fix: route Nemotron 3.5 to us-north1 (per-model base URL)

- **PR:** SunnyBagal/TraceHound#55 (draft), branch `fix/model-base-url` from `origin/main` @ `7de5dbd`
- **Head sha:** `5ec764dba47ec2cbbcd2b2ae2977e804c8f87957`
- **CI:** `test-and-build` pass (first run, no fix needed); `review` skipped.

## Commits
- `1379508` llm client: route Nemotron 3.5 Super VL to us-north1 per request
- `5a6d343` llm client: tests that routing leaves the cache key and ledger alone
- `1ec811a` reviewbench: log the resolved base URL; document 3.5 on us-north1
- `5ec764d` feedback: Token Factory serves 3.5 Super VL only from us-north1

## Tests
- Step 0 (before changes): GIT_* env check empty; `pnpm test` green (action 226, analyzer 859 + 7 skipped, viewer 188); `pnpm typecheck` green.
- After: `pnpm test` green: action 226, analyzer 869 passed + 7 skipped (10 new client tests), viewer 188. `pnpm typecheck` green.
- Adapter: `pnpm exec vitest run --config scripts/reviewbench/vitest.config.ts` 47 passed (3 files, 1 new test with 5 cases); `pnpm exec tsc -p scripts/reviewbench` clean.
- Frozen prompt sha256 pins: pass (part of `pnpm test`); no prompt touched.

## What was built
- `client.ts`: exported `MODEL_BASE_URLS` (one entry: `nvidia/Nemotron-3.5-Super-VL` → us-north1) and `baseUrlFor(model, configured)`. A request uses the table URL only when the configured host matches `api.tokenfactory.<region>.nebius.com` and the model id matches exactly (case-sensitive, own keys only). Otherwise it uses the configured URL unchanged. `GET /models` stays on the configured URL.
- Tests (fake fetch, URL asserted): 3.5 with the default base goes to us-north1; 3 Super with the default base stays on us-central1; 3.5 with a us-central1 base goes to us-north1; non-Nebius hosts (models.example.invalid, 127.0.0.1, a look-alike suffix host, an invalid URL) are unchanged; an unknown model, a different case, or `constructor` is unchanged.
- `entry.ts` prints `base_url=` as the client resolves it for `RB_CONFIG_MODEL`. `NEBIUS_BASE_URL` passed to the stages is unchanged.
- `docs/bench/reviewbench.md`: routing documented; the "Other egress hosts" row names `api.tokenfactory.us-north1.nebius.com` (already registered, review-bench/ReviewBench#92). Smoke workflow: one comment line, `MODEL_BASE_URL` still us-central1.
- `FEEDBACK.md`: dated entry with only the given facts; the opinion is labelled; no key.

## Cache key
- The cache key is `sha256(model + "\0" + JSON(request))`. It never contained the base URL, and it did not change; `cache.ts` was not touched. A pinned test gives the 3 Super key for a fixed request, and a pre-routing entry is still served offline under the default, us-central1 and us-north1 bases. Two models from one client hit two URLs and write two cache entries. The ledger records no URL. No cached response format changed.

## Not done / notes
- `packages/analyzer/test/helpers.ts` got a `baseUrl` option on `fakeClient` (under `packages/analyzer/test/`, allowed).
- No Nebius or model calls were made this session. No merge, no force-push.
