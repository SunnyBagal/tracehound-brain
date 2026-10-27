Audited fresh clone of `SunnyBagal/TraceHound`: main `5ba60a01c1c4fbac1d81b990a3198ad2a66d6fa5`; draft `docs/judges-readme` at `2bb314494bc15b2f4a719cf407ee2715a813c91f` (PR #54), saved as `_judge/README.md`.

Every command used `env -u NEBIUS_API_KEY -u NVIDIA_API_KEY` (omitted below for space). Evidence gives exit code, wall seconds, and first error; “none” means no error output. Exact commands/logs are in `_judge/commands.jsonl` and `_judge/logs/`. No model calls.

Confinement caveat: pnpm ignored the attempted store-directory environment override and reported reusing `/Users/sunny/Library/pnpm/store/v11`; I cannot certify absence of writes there. A workspace-level `.pnpm-store` created during the audit was moved under `_judge/`. Final workspace status contains only untracked `_judge/`.

| Check | Result | Evidence (command; exit; seconds; first error) |
|---|---|---|
| Fresh clone | pass | `git clone https://github.com/SunnyBagal/TraceHound _judge/repo`; sandbox 128, 0.064s, `Could not resolve host: github.com`; network retry 0, 2.014s, none. |
| Draft/main | pass | `git show origin/docs/judges-readme:README.md > ../README.md`; 0, 0.042s, none. `git merge-base --is-ancestor 5ba60a0 origin/main`; 0, 0.018s, none. |
| Node | pass | `node -v`; 0, 0.030s, none: **v24.14.1**. |
| pnpm | pass | `pnpm -v`; sandbox 1, 0.220s, registry `ENOTFOUND`; network retry 0, 1.518s, none: **11.20.0**. Corepack downloaded the pinned pnpm locally. |
| Version agreement | pass | README L260: Node ≥24/pnpm 11; root engines ≥24; viewer engines 24.x; `.nvmrc` 24; no `.node-version`. Both Node-using workflows set **24**, not the supplied Node-22 note. `git --version`: 0, 0.009s, git 2.50.1. |
| Install | pass | `pnpm install`; sandbox interrupted at exit 130 after ~118.431s of registry `ENOTFOUND` retries; network retry 0, 4.244s, none. Lockfile unchanged; `--frozen-lockfile` was not needed. |
| Fixture | pass | `pnpm fixture`; sandbox 128, 0.429s, GitHub DNS failure; network retry 0, 2.032s, none. Fixture at `da0e3d640a9c02f815fcca48f8328c94558cc058`. |
| Gate environment | pass | Required `env`/`grep` check printed nothing before tests, typecheck, each build attempt and toy build; grep exit 1, 0.014–0.074s, none. Gates ran outside git hooks. |
| Tests | pass | `pnpm test`; 0, 43.512s, none. No 5-second timeout failures; no reruns needed. Exact counts below. |
| Typecheck | pass | `pnpm typecheck`; 0, 9.747s, none; all three packages passed. |
| Viewer production build | fail | `pnpm --filter @tracehound/viewer build`; 1, 20.289s, `Warning: Error while requesting resource` (Google Fonts downloads failed). Two elevated retries: 1, 2.411s and 1, 1.842s; first error `FATAL: An unexpected Turbopack error occurred`, ultimately `binding to a port / Operation not permitted`. Production export unverified; environment restriction. |
| README dev server | pass | `pnpm --filter @tracehound/viewer dev`; sandbox 1, 1.206s, `Failed to start server` / `listen EPERM`. Elevated retry became ready; intentionally stopped with Ctrl-C, exit 130, 157.051s, no error. |
| Local `/` | pass | `curl -s -o /dev/null -w '%{http_code}' http://localhost:3000/`; sandbox 7, 0.014s, HTTP 000 with no error text; elevated 0, 1.800s, HTTP **200**, none. |
| Local `/graph` | pass | Same curl against `/graph`; 0, 2.362s, HTTP **200**, none. |
| Local `/reviews` | pass | Same curl against `/reviews`; 0, 0.318s, HTTP **200**, none. |
| Local `/benchmark` | pass | Same curl against `/benchmark`; 0, 0.207s, HTTP **200**, none. Server stopped afterward. |
| No-key demo | fail | `pnpm demo`; **0**, 1.776s: no fail-fast error. Diagnostic: `../../.env not found. Continuing without it.`, then `[naming] NEBIUS_API_KEY not set; using heuristic names`. It overwrote tracked snapshots, as README L275–278 discloses. |
| Snapshot cleanup | pass | `git status --porcelain snapshots/`; 0, 0.012s: modified `snapshots/da0e3d640a9c02f815fcca48f8328c94558cc058/0.10.0.json` and `snapshots/index.json`. `git checkout -- snapshots/`: 0, 0.016s; subsequent status 0, 0.016s, empty. |
| Toy repository | pass | `node eval/fixtures/build-toy-repo.ts`; 0, 0.072s, none; built toy-cart at `3582f355e884dc4512ee157ee59848796893ec27`. |
| Docker prerequisite | pass | `docker version`; sandbox 1, 0.123s, `permission denied while trying to connect to the docker API`; elevated read-only retry 0, 0.065s, none: client/daemon 29.8.0. Running Docker is stated at L263–264 and L293–295. |
| Toy noop completion | not run | `pnpm tracehound repair --task eval/tasks/toy-discount/task.json --agent noop --provider docker`; sandbox attempt **3**, 0.600s, `Docker is not available: permission denied ... docker.sock`. Full elevated run skipped because it would write Docker-managed storage outside the allowed workspace. No RESOLVED/UNRESOLVED outcome obtained. |
| Frozen Action ref | pass | `git rev-list -n 1 review-freeze-2`; 0, 0.016s, none: exactly `0aeef06fa8742cc8a36581c3894e554b8748c965`. |
| Action inputs/defaults | pass | `ruby ../action-check.rb`; 0, 0.093s, none. Accepts both `nebius-api-key` and `model`; no missing required input. Omitted defaults: reviewer `pool-verify-v1`, max-comments `15`, fixes `on`, learn `off`, contracts `on`, dry-run `false`, data-branch `off`. |
| YAML syntax | pass | `ruby -ryaml -e 'ARGV.each { \|f\| YAML.parse_file(f); puts "parsed #{f}" }' ../try-workflow.yml ../pinned-action.yml ../example-workflow.yml`; 0, 0.168s, none: all three parse. Used installed Ruby Psych; no dependency installed. Requested Python alternative: `python3 -c 'import yaml; print(yaml.__version__)'`, 1, 0.028s, `ModuleNotFoundError: No module named 'yaml'`; no standalone Node YAML parser found. |
| Example differences | pass | Same comparison command, 0, 0.093s: removes `data` job and `push` trigger; pins Action SHA instead of example `@main`; explicitly supplies model instead of inheriting default; omits example's max-comments/dry-run/data-branch values, all equal to pinned defaults. Review job otherwise structurally identical. |
| Action missing-key guard | pass | `node packages/action/src/cli.ts`; **1**, 0.154s, `TraceHound: NEBIUS_API_KEY is missing. Set the required nebius-api-key action input.` One attempt only. |
| Actual Action review | not run | needs a Nebius key (not run). No workflow installed or GitHub review posted. |
| Relative links/anchors | fail | `python3 ../links.py`; 1, 0.757s. **23 occurrences / 20 unique targets; two broken occurrences:** L77 and L248, `docs/bench/results.md#7-precision-levers-tried-on-dev`, absent on both refs. Heading is section **6**, confirmed with `git grep` (0, 0.020s). All other relative links/anchors resolve. |
| HTTPS: demo repository | pass | `curl -sI -L https://github.com/SunnyBagal/tracehound-demo`; 0, 0.941s, **200**, none. |
| HTTPS: historical results | pass | `curl -sI -L 'https://github.com/SunnyBagal/TraceHound/blob/e91a268/docs/bench/results.md#5-all-50-prs'`; 0, 0.878s, **200**, none. HEAD does not validate this external fragment. |
| HTTPS: React Flow | pass | `curl -sI -L https://reactflow.dev`; 0, 0.755s, **200**, none. |
| HTTPS: ELK | pass | `curl -sI -L https://eclipse.dev/elk/`; 0, 1.304s, **200**, none. |
| HTTPS: elkjs | pass | `curl -sI -L https://github.com/kieler/elkjs`; 0, 1.254s, **200**, none. |
| HTTPS: svgl | pass | `curl -sI -L https://svgl.app`; 0, 1.074s, **200**, none. Each external URL requested once. |
| Live `/` | pass | `curl -s -o ../live-.html -w '%{http_code}' https://tracehound-tau.vercel.app/`; 0, 1.042s, **200**, none. |
| Live `/graph` | pass | Same curl against `/graph`, response saved under `_judge/`; 0, 0.717s, **200**, none. |
| Live `/reviews` | pass | Same curl against `/reviews`; 0, 0.832s, **200**, none. |
| Live `/benchmark` | pass | Same curl against `/benchmark`; 0, 0.982s, **200**, none. Still displays **v1**: headline F 58.3%, all-50 rank 4th; expected pending redeploy. |
| Draft completeness | fail | README L153 and L186 contain literal `[[TBD-SEEDED]]`. L184–186 says the current demo config posted no findings; older reviews are from other configs. |
| Final tracked state | pass | Fresh clone `git status --porcelain`: 0, 0.031s, empty. No project commits, additional branches or PRs; no lasting tracked-file edits. Unfinished: production build, complete Docker noop, model-backed steps. |

Judge-reading findings:

- **L5–42:** the first “Try it” requires a paid Nebius key; no key-free path appears on the first screen. The local fixture/test route is buried at L266–273.
- **L7–8, L21, L45–46:** trying the Action assumes control of repository secrets and a same-repository PR; a judge using a fork gets no review. The fork limitation is stated after the workflow.
- **L10–42, L48, L79:** the large workflow fills the opening screen before the explanation and results; there is no immediate output preview or results pointer beside the opening description.
- **L79, L181–186:** results and the demo link appear much later. The demo links to a repository; the deployed dashboard URL is absent from the README.
- **L174–178:** cost and partial-review behavior are distant from the paid “Try it” instructions; about 7/40 PRs per dev run lose a chunk to invalid JSON.

Exact step-2 counts (original run; no timeout reruns):

| Scope | Passed | Skipped | Failed | Total |
|---|---:|---:|---:|---:|
| Action tests | 237 | 0 | 0 | 237 |
| Analyzer tests | 969 | 7 | 0 | 976 |
| Viewer tests | 188 | 0 | 0 | 188 |
| **All tests** | **1,394** | **7** | **0** | **1,401** |
| Test files | 94 | 5 | 0 | 99 |
| Package typechecks | 3 | 0 | 0 | 3 |
