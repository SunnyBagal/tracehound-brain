# rb-01: package TraceHound for ReviewBench (adapter + container image + smoke test)

Tool: **Claude cloud** (claude.ai/code). Start a **new session** on repo `SunnyBagal/TraceHound`. It spends cloud credit; the smoke test spends a few cents of Nebius through GitHub Actions. Paste everything below the line.

---

You're packaging TraceHound's existing reviewer as a ReviewBench agent: a `linux/amd64` container image, published on ghcr.io, that follows ReviewBench's agent contract. **You're not changing how the reviewer reviews.** Prompts, models and pipelines stay exactly as they are on main.

## Read first
- `AGENT_CONTRACT.md`, `docs/ONBOARDING.md`, `docs/RUNNER.md` and `examples/` in `https://github.com/review-bench/ReviewBench` at commit `12e9d1a` (MIT). Clone it into your scratch space, not into our repo.
- **Blindness, a hard rule:** never open, read, grep or copy anything under ReviewBench's `golden/` directory, and never look at its judge or classifier prompts. Our submission config is pre-registered (see the end of this brief). A session that reads the answer key voids the result.
- Our side: `packages/action/src/pipeline.ts` (pool-verify-v1: diff-v1 + ctx-v1 → pool → verify-ctx-v1), `packages/analyzer/src/llm/client.ts` (Nebius base URL from `NEBIUS_BASE_URL`), and draft PR #42's `scripts/reviewbench/`: `adapter.ts` and `format.ts` (a spike written without the contract; fix it against the contract).

## Step 0 (if any check fails, STOP and report)
- Branch `rb/image-v1` from `origin/main`, which contains `9a81505`. `pnpm install --frozen-lockfile`, `pnpm test` and `pnpm typecheck` are green.
- **Don't open** `SunnyBagal/linea-seeded`, `recall-seeded` or `tracehound-seeds`.

## Build (one commit per item; tests green; stage by explicit path)

1. **`scripts/reviewbench/entry.ts`: the container entrypoint.**
   - Read the contract inputs: `/work/repo` (the head checkout), `/work/pr/diff.patch`, `/work/pr/pr.json`, `RB_BASE`, `RB_HEAD`, `RB_PR_NUMBER`, `RB_OUT`, `RB_AGENT`.
   - Build our `ReviewerInput` from them (title, body, number, diff), then run the pipeline in-process, with the repo dir set to `/work/repo`.
   - **`RB_CONFIG_PIPELINE` selects the pipeline:**
     - `pool-verify-v1`: what the Action runs today;
     - `pool-raw`: diff-v1 + the ctx-only findings, no verifier. This is pool-verify-v1 with the last stage skipped; reuse the same functions;
     - `pool-raw-x2`: pool-raw run twice with the same prompts, then the union, deduplicated with `scripts/bench/agree-rule.ts` `agrees` (keep the first).
     - No other values. An unknown value exits non-zero with a clear message.
   - `RB_CONFIG_MODEL` picks the model id. The default is the one pool-verify-v1 uses today. Print the settings applied, e.g. `pipeline=pool-raw model=…`, because the contract requires it.
   - **Output** `findings.json` exactly per the contract:
     - `pr` holds repo, pr_number, base, head; `head` must equal `RB_HEAD`.
     - Each finding has `file`, `start_line`, `end_line`, `message` and `producer` = `RB_AGENT`.
     - Line numbers are at head. Use the finding's line for both start and end when there's no range. Drop findings with a null line, and count them.
   - **It must never fail a PR.**
     - Wrap the whole pipeline. On any error, timeout or invalid model output after the existing retries, write a valid file with `"findings": []`, log the reason to stderr, and exit 0.
     - Set a self-imposed wall-clock limit of 25 minutes, under the 30-minute default.
     - Cap findings at 40 per PR, keeping pipeline order.
     - Validate the JSON before exiting.
     - The contract says one failed PR fails the whole run, so robustness matters more than anything else here.
   - The network allows only the Nebius host. There's no GitHub API inside. Make sure nothing in the pipeline calls GitHub; the Action-only paths, such as posting and fix suggestions, must not run.
2. **`Dockerfile.reviewbench`** (repo root):
   - `--platform=linux/amd64`, Node 24, pnpm via corepack, `pnpm install --frozen-lockfile --prod=false`, copy only what the pipeline needs, `ENTRYPOINT` runs `entry.ts` with Node's native TS support.
   - `git` must be in the image (ctx-v1 uses `git grep`).
   - No secrets baked in. `NEBIUS_API_KEY` comes from the run's secrets, and `NEBIUS_BASE_URL` from `RB_MODEL_BASE_URL` when set.
3. **`.github/workflows/reviewbench-image.yml`:** build and push `ghcr.io/sunnybagal/tracehound-reviewbench`, on `workflow_dispatch` and on pushes to `rb/**` branches, using ReviewBench's onboarding example (`GITHUB_TOKEN`, `packages: write`). Print the digest in the step summary. Make the package **public**, so no pull token is needed; if the workflow can't set that, say so and Sunny will do it.
4. **`.github/workflows/reviewbench-smoke.yml`:** `workflow_dispatch` only.
   - It clones ReviewBench at `12e9d1a` and runs `scripts/try-agent.sh <image@digest> --pr <index>` for **two** PRs from their 25-PR test set: index 0, plus one in a non-TS language.
   - Pass `-e NEBIUS_API_KEY` from repo secret `NEBIUS_API_KEY`, `-e RB_MODEL_BASE_URL=<our base URL>`, and `-e RB_CONFIG_PIPELINE=pool-raw`.
   - It uploads the findings files as an artifact. It must **not** run their judge, and must not read `golden/`.
5. **Tests:**
   - unit tests for `entry.ts`: input mapping, output schema, the never-fail path writing `[]` on a thrown error, the cap, null-line drops, and an unknown pipeline value;
   - a fixture `pr.json` and `diff.patch` built by hand, not from ReviewBench data.
6. **Docs:** `docs/bench/reviewbench.md` covers what the image does, the three configs, the never-fail policy, the blindness rule, and the cost estimate.
   - Cost: about 657 PR runs (219 × 3) × our per-PR cost. Use `results.md`'s pipeline cost: median $0.0317, max $0.23 for pool-verify-v1. pool-raw skips the verifier.

## Rules
- Touch only `scripts/reviewbench/`, `Dockerfile.reviewbench`, `.dockerignore` (if needed), `.github/workflows/reviewbench-*.yml`, `docs/bench/reviewbench.md`, and tests under `scripts/reviewbench/`.
- **Don't change** `packages/` (prompts, pipelines, Action), `config/` or `scripts/bench/`. If the pipeline can't run in-process without a change there, STOP and report what's needed.
- No force-push, no merge. Open the PR to main **as a draft**, titled `rb-01: ReviewBench agent image (adapter, Dockerfile, build + smoke workflows)`. #42 can be closed after this merges; say whether anything in it is still needed.
- Sunny will run the smoke workflow once the image builds. You can trigger it yourself if the session can dispatch workflows. Either way, report the findings counts per PR and the wall time, never scores.

End with: the PR number, the head sha, one line per commit, the test count, the image digest (if built), the smoke results (findings per PR, wall time, exit codes), and the exact values Sunny should enter in the ReviewBench portal (image digest, model API URL, secret names, timeout).
