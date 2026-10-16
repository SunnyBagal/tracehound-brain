# Decision digest

One entry per repo decision, extracted from `docs/decisions.md` on main (`c34b2ed`, 2026-10-16). The date is when the entry first appeared in git. This is a search aid: the repo entry (linked) is the source of truth. Regenerate after new decisions land; don't hand-edit old entries.

### 001 · Run TypeScript with Node's native type stripping
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#001--run-typescript-with-nodes-native-type-stripping)
- **Choice:** Node 24 runs `.ts` directly (`node src/cli.ts`). The code uses `.ts` import extensions and `erasableSyntaxOnly`.
- **Rejected:** `tsx`/`ts-node`. Each is one more dependency and a second TS semantics to keep in sync with. The cost of our choice is no enums or parameter properties, which we don't need.

### 002 · TypeScript 6.x for type-checking, not 7.x
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#002--typescript-6x-for-type-checking-not-7x)
- **Choice:** `typescript ~6.0` for `tsc --noEmit` and Next.js. ts-morph bundles its own compiler, so the analyzer's parsing does not depend on this version.
- **Rejected:** TS 7 (the native Go port). It is the npm `latest`, but Next.js and other tooling still expect the JS compiler API.

### 003 · Group by anchor reachability, not by directory or community detection
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#003--group-by-anchor-reachability-not-by-directory-or-community-detection)
- **Choice:** Anchors are files that mark a responsibility: entry points, modules that register routes, and modules that wrap an infra client. Each anchor claims the files that only it can reach through resolved imports.
- **Rejected:** (a) Directory grouping. Layered repos (`routes/`, `controllers/`, `utils/`) cut across features, so it would output "Controllers", not "Exchange API". (b) Louvain/label propagation.

### 004 · One Redis node per connection, keys listed on the node
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#004--one-redis-node-per-connection-keys-listed-on-the-node)
- **Choice:** Redis clients are grouped by connection (the env var or URL they connect with). Queue keys appear as edge labels and node metadata.
- **Rejected:** One node per key. The response queue key is `response-queue-${uuid}` on the backend and `message.responseQueue` (runtime data) in the engine.

### 005 · Queue edges follow data flow; everything else follows dependency
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#005--queue-edges-follow-data-flow-everything-else-follows-dependency)
- **Choice:** producer → queue → consumer (`produces`/`consumes`). Imports, KV reads/writes and DB queries point from the dependent component to its dependency.
- **Rejected:** Pure dependency direction everywhere (consumer → queue). The canvas would lose the request/response pipeline, which is the most useful thing it shows about a queue-based system.

### 006 · Embed code snippets in the snapshot
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#006--embed-code-snippets-in-the-snapshot)
- **Choice:** Each evidence item carries its line range plus ±2 lines of context, capped at 14 lines. A snapshot is self-contained and immutable per (commit SHA, analyzer version).
- **Rejected:** Reading files at request time. The API would need a checkout of every analyzed commit, and a snapshot could drift from its snippets.

### 007 · Resolve static values with decaying confidence
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#007--resolve-static-values-with-decaying-confidence)
- **Choice:** A small evaluator follows const variables, const object properties, env-helper calls and `x ?? "default"` across imports. Confidence is 1.0 for literals, 0.9 for provenance-backed patterns, 0.7 through fallbacks/indirection, and 0.5 when the value is dynamic.
- **Rejected:** Dropping unresolved ops. The engine's `lPush(message.responseQueue)` is how responses reach the backend, and hiding it would erase half the round trip.

### 008 · Client-only modules fold into their resource node
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#008--client-only-modules-fold-into-their-resource-node)
- **Choice:** A module whose exported values are all client instances (`db.ts` exporting `prisma`) belongs to the DB/Redis node. Modules that wrap a client in behavior (`engine-client.ts`) are their own service component.
- **Rejected:** Always a separate component per client module. It creates a "db.ts" box that only duplicates the database node.

### 009 · Static snapshot files + manifest instead of an API (hackathon scope)
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#009--static-snapshot-files--manifest-instead-of-an-api-hackathon-scope)
- **Choice:** The analyzer writes `snapshots/<sha>/<analyzerVersion>.json` and upserts `snapshots/index.json`, which lists every snapshot (repo, sha, analyzerVersion, path, createdAt) and has a `latest` pointer. The viewer fetches the manifest, then the snapshot, as plain static files.
- **Rejected:** A Fastify `GET /snapshot/:sha` server. It adds a process to run, CORS or rewrite plumbing, and a deploy target, and gives nothing a static file doesn't while snapshots are immutable and produced offli…

### 010 · tracehound.json overrides pin files and then act like anchors
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#010--tracehoundjson-overrides-pin-files-and-then-act-like-anchors)
- **Choice:** The config maps a component id to file globs (or `{ name, kind, files }`). Matched files are pinned before any heuristic runs. The key becomes the component id verbatim. Pinned components are never merged, split or renamed. When a file matches two overrides, the first key wins and the membership reason records it.
- **Rejected:** (a) Overrides that only rename heuristic components. Renames can't split or merge, and the ids would still depend on heuristics. (b) Pin-only overrides with no reach.

### 011 · Categorical confidence labels on edges; numbers stay internal
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#011--categorical-confidence-labels-on-edges-numbers-stay-internal)
- **Choice:** Each evidence item records how its operand was established: `proven` (a literal or compiler-resolved target), `resolved-default` (taken from a `?? "default"` fallback, e.g.
- **Rejected:** (a) Showing the number. "0.7" means nothing to a reader, and the numbers mix two axes (provenance strength vs. value resolution). (b) Deriving labels from numeric bands.

### 012 · LLM naming is a separate, facts-only, fail-safe pass
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#012--llm-naming-is-a-separate-facts-only-fail-safe-pass)
- **Choice:** After deterministic analysis, `nameComponentsWithLlm` sends each component's extracted facts to Nemotron Nano (`nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B`, Nebius Token Factory, OpenAI-compatible `/chat/completions`, temperature 0).
- **Rejected:** (a) Sending source code or snippets. That costs more tokens, invites the model to "discover" relationships, and leaks code to a third party for little naming gain.

### 013 · Hard budget caps enforced before each call, priced pessimistically
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#013--hard-budget-caps-enforced-before-each-call-priced-pessimistically)
- **Choice:** Every model call goes through `TokenFactoryClient`, which does the following: - **Estimate before sending.** It prices a worst case: ~3 chars/token for the prompt plus the full `max_tokens` of output, from `config/prices.json`.
- **Rejected:** (a) Checking spend only after calls. By then the money is gone, and with 4 concurrent calls the overshoot is 4×. (b) Silently falling back to heuristic names when the budget is hit.

### 014 · Content-addressed response cache
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#014--content-addressed-response-cache)
- **Choice:** Successful completions are stored in `.tracehound/cache/<sha256(model + full request JSON)>.json`. The key covers the system prompt, the facts payload, temperature and max_tokens. Hits make no request, write no ledger entry and cost $0.
- **Rejected:** (a) A cache keyed by commit SHA + component id. It serves stale names after a prompt or grouping change. (b) Storing names in the snapshot and reusing them.

### 015 · Left-to-right layered layout (ELK `layered`, direction RIGHT)
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#015--left-to-right-layered-layout-elk-layered-direction-right)
- **Choice:** ELK's layered algorithm, flowing left to right with 140px between layers. Requests enter at the left (server entry → APIs), cross the middle (RPC bridge, Redis) and reach the worker and data stores on the right, which is how people read a request path.
- **Rejected:** (a) Top-to-bottom. It is taller than the viewport on laptops, and the edge labels ("produces · resolved-default") are wide and collide more when stacked vertically. (b) A force-directed layout.

### 016 · Floating edges instead of fixed left/right handles
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#016--floating-edges-instead-of-fixed-leftright-handles)
- **Choice:** Each edge leaves from whichever side of its source faces the target and enters the facing side of the target, as a cubic bezier. Edges between the same two nodes (in either direction) get their own lane, 30px apart along that side.
- **Rejected:** Fixed source-right / target-left handles, the React Flow default. The graph has real cycles (Redis RPC Bridge ↔ Redis ↔ Engine Worker, Bridge ↔ Pending-Response Registry), so every backward edge loope…

### 017 · Confidence is a dash pattern plus text, never style alone
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#017--confidence-is-a-dash-pattern-plus-text-never-style-alone)
- **Choice:** proven = solid, resolved-default = dashed (7 5), dynamic = dotted (round caps). Every edge label reads `kind · label` ("consumes · dynamic"). The inspector and the top-bar legend show the same line sample next to the word.
- **Rejected:** Colour-coding confidence. It fails for colour-blind users and in greyscale screenshots, and colour is already used for hover/selection.

### 018 · Viewer serves copied snapshots; the repo's `snapshots/` stays the source of truth
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#018--viewer-serves-copied-snapshots-the-repos-snapshots-stays-the-source-of-truth)
- **Choice:** `snapshots/` is committed at the repo root, where the analyzer writes. The viewer's `build`/`dev` scripts copy `index.json` plus only the snapshot files it references into `viewer/public/snapshots` (gitignored), so the static export fetches them with relative URLs.
- **Rejected:** (a) Importing the snapshot JSON into the JS bundle. Adding a snapshot would need a rebuild of the code rather than just the data, and the manifest indirection would be pointless.

### 019 · Vercel Root Directory = `viewer`, with files outside the root included in the build
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#019--vercel-root-directory--viewer-with-files-outside-the-root-included-in-the-build)
- **Choice:** The Vercel project builds from `viewer/` and reads `../snapshots` and `../packages/analyzer` at build time. That relies on Vercel's "Include files outside the root directory in the Build Step" (Project Settings → Build and Deployment → Root Directory).
- **Rejected:** (a) Committing a second copy of the snapshots inside `viewer/public/`. That duplicates data in git, and the two copies drift. (b) Fetching snapshots from raw.githubusercontent.com at runtime.

### 020 · Naming runs with reasoning off (`chat_template_kwargs.enable_thinking=false`)
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#020--naming-runs-with-reasoning-off-chat_template_kwargsenable_thinkingfalse)
- **Choice:** Naming requests send `chat_template_kwargs: {"enable_thinking": false}`. `--reasoning on` restores the model default. On the demo's 7 components (same facts, Nano) this cut completion tokens from 2,021 to 229 (8.8×), cut per-call latency from 1.4–2.8 s to 0.9–1.5 s, and cut cost from $0.00062 to $0.00019 per run.
- **Rejected:** (a) `reasoning_effort: "none"`, the parameter the Token Factory OpenAPI spec documents. On Nano it returned `content: null` with the JSON answer inside `message.reasoning`, which breaks standard OpenA…

### 021 · Deterministic checks on model names before they're stored
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#021--deterministic-checks-on-model-names-before-theyre-stored)
- **Choice:** Every reply is Unicode-normalized to ASCII punctuation, then must pass two checks: identifier-like tokens in the summary must exactly match extracted facts, and the name must not describe just one route or file of a multi-part component.
- **Rejected:** A second model call to judge the first. It doubles cost, it's non-deterministic, and it can hallucinate too. Rules are cheaper and testable against the real bad outputs.

### 022 · Drive Nebius Sandboxes from TypeScript over REST, no Python bridge
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#022--drive-nebius-sandboxes-from-typescript-over-rest-no-python-bridge)
- **Choice:** Sandboxes (Contree) have a documented REST API with a published OpenAPI 3 spec (`https://eu-north.nebius.computer/static/api.yaml`, base `https://api.tokenfactory.nebius.com/sandboxes/v1`): - `POST /instances` spawns a run, with `disposable`, `timeout`, `networking.enabled`, `env` and `files`.
- **Rejected:** A thin Python bridge around `contree-sdk` / `contree-client`. The SDK adds only convenience (image objects, branching helpers) over the same REST calls.

### 023 · Impact analysis: which way to walk each edge kind
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#023--impact-analysis-which-way-to-walk-each-edge-kind)
- **Choice:** `tracehound impact` maps changed files to components through the **base** commit's snapshot, then walks component edges breadth-first up to `--depth` (default 2): - `imports`, `queries`: **reverse**.
- **Rejected:** (a) Forward-only walking (follow imports from the change outward): that answers "what does this code use", not "what could this break".

### 024 · Crossing a queue costs one hop of impact depth
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#024--crossing-a-queue-costs-one-hop-of-impact-depth)
- **Choice:** Entering a broker component (`queue` or `cache` kind: a Redis resource) costs one hop; leaving it costs nothing. Producer → broker → consumer is one hop of `--depth`, the same as an import.
- **Rejected:** (a) Raising the default depth to 3: every import chain would get one hop longer to fix a queue-only problem. (b) Dropping broker nodes from the chain: it would hide which queue couples the two sides,…

### 025 · Agent context packets: deterministic lexical ranking v1, heuristic confidence
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#025--agent-context-packets-deterministic-lexical-ranking-v1-heuristic-confidence)
- **Choice:** `tracehound context`, `tracehound query` and `tracehound mcp` read one snapshot file and nothing else: no network, no model. The ranking was written down before running it on any real issue, and isn't tuned to specific strings.
- **Rejected:** (a) Embeddings or an LLM re-ranker: not deterministic, needs network and spend, and can't explain its ranking. (b) Using model-written summaries as ranking text: they contain unverified claims ("valid…

### 026 · Repair harness core: provider interface, harness-owned verification, Docker first
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#026--repair-harness-core-provider-interface-harness-owned-verification-docker-first)
- **Choice:** Feature 6 starts with the harness, not the agent. - **Provider interface:** `SandboxProvider` is deliberately small: `create({ image, source, network? })`, `exec(handle, cmd, { timeoutMs })`, `writeFile`, `readFile`, `destroy`. - `source` is `{ gitUrl, sha }` or `{ localPath }` (localPath is for test fixtures only).
- **Rejected:** (a) `git clone --depth 1`: a shallow clone still carries the real commit, its message and author, and `.git/shallow` names the cut.

### 027 · Decider interface; Nemotron localizer over deterministic facts, with a visible fallback
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#027--decider-interface-nemotron-localizer-over-deterministic-facts-with-a-visible-fallback)
- **Choice:** Localization (which components an issue is about) goes through a `Decider`: `decide({ issue, snapshot, k }) → { ranking: [{ componentId, reason }], decider, usage }`. - `lexical` is ranking v1 (025), unchanged.
- **Rejected:** (a) Merging the model's picks with lexical ones: nobody could tell which ranking produced a given component. (b) Giving the model source code or the naming summaries: that's more tokens, and the summa…

### 028 · Viewer: fact-backed tech icons, overlay inspector with history-backed navigation
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#028--viewer-fact-backed-tech-icons-overlay-inspector-with-history-backed-navigation)
- **Choice:** **Icons come from facts, in the viewer.** `viewer/lib/tech.ts` picks a technology icon only from deterministic snapshot facts, never from `name`/`summary`: PostgreSQL when the Prisma datasource provider (already a file fact with evidence, `prismaDatasource`) is `"postgresql"`; Prisma for Prisma data stores;
- **Rejected:** (a) A `tech` field in the analyzer for all icons: the facts already exist with evidence, and a schema change would have meant a version bump and regenerating snapshots and impacts for no new informati…

### 029 · Repair agent loop: host-side TypeScript, native tool calls
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#029--repair-agent-loop-host-side-typescript-native-tool-calls)
- **Choice:** **Protocol choice: native OpenAI-style `tool_calls`.** The spike (FEEDBACK 2026-09-30, 3 calls) showed Nemotron Nano on Token Factory returning `message.tool_calls` with parseable JSON `arguments`, reasoning on or off, and continuing correctly after `role: "tool"` results. With reasoning on, the thi
- **Rejected:** (a) A JSON-action protocol in `content`: that means more parsing code and more failure modes, for no benefit once native calls work.

### 030 · Base agent competence: agent-v2 (same in both graph conditions)
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#030--base-agent-competence-agent-v2-same-in-both-graph-conditions)
- **Choice:** the first toy-cart transcripts (agent-v1, 10 runs in `runs/session*`, 5 of them UNRESOLVED at the 40-step limit) failed for generic reasons, not for lack of architecture knowledge: the model repeated near-identical calls (about 30 invented `bun test --trace-*` flags;
- **Rejected:** (a) Detecting the environment at run time (`which …`): that is more commands per run for a fixed, pinned image. (b) Blocking the 2nd identical call: re-reading a file once is normal.

### 031 · Viewer: capped zoom, a header icon rule, transient panel↔canvas highlight, left rail
- **Date:** 2026-09-30 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#031--viewer-capped-zoom-a-header-icon-rule-transient-panelcanvas-highlight-left-rail)
- **Choice:** **Zoom is capped at 0.4–1.5 on an unbounded canvas** (Railway-style): panning is free, zoom is not. The limits live in `viewer/lib/zoom.ts` and apply to the React Flow props, both fit views (after layout, and the Fit view button), the warnings focus pan and the keep-selection-visible pan (`keepInView`, which pans at th…
- **Rejected:** (a) The old 0.2–2 range: node text is unreadable well before 0.2 (the phone fit landed at 0.22), and at 2 one node fills most of a laptop screen.

### 032 · Git env hygiene: every git spawn drops the repo-selecting GIT_* variables
- **Date:** 2026-10-01 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#032--git-env-hygiene-every-git-spawn-drops-the-repo-selecting-git_-variables)
- **Choice:** `packages/analyzer/src/git-env.ts` (no imports; also exported as `@tracehound/analyzer/git-env`): `cleanGitEnv(env)` returns a copy without `GIT_DIR`, `GIT_WORK_TREE`, `GIT_INDEX_FILE`, `GIT_OBJECT_DIRECTORY`, `GIT_ALTERNATE_OBJECT_DIRECTORIES`, `GIT_COMMON_DIR`, `GIT_NAMESPACE`, `GIT_PREFIX`, `GIT_CONFIG_PARAMETERS`,…
- **Rejected:** (a) Fixing only the test helpers: `impact --diff` and `analyze` from a hook would still read the wrong repo. (b) Passing `--git-dir`/`--work-tree` explicitly everywhere: it's easy to forget on one cal…

### 033 · agent-v3: output cap, stuck stop, one no-edit nudge, one way to run scratch code
- **Date:** 2026-10-01 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#033--agent-v3-output-cap-stuck-stop-one-no-edit-nudge-one-way-to-run-scratch-code)
- **Choice:** agent-v2 failed the toy gate (graph off 2/5; decision 030 addendum, `eval/dev-log/2026-09-30-toy-gate.md`). The failed runs were dominated by `bun run -e`, whose ~1,900-token usage text filled the context; by the model re-sending a refused call for up to 31 turns; and by never editing. v3 changes only these.
- **Rejected:** (a) Stopping on the first refusal: the model sometimes recovers after one refusal. (b) Repeating the nudge: a nudge every N steps becomes noise, and v3 is meant to measure one intervention.

### 034 · agent-v4: reasoning on by default, /scratch outside the repo, base-file edit accounting, agent tests out of the verdict
- **Date:** 2026-10-01 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#034--agent-v4-reasoning-on-by-default-scratch-outside-the-repo-base-file-edit-accounting-agent-tests-out-of-the-verdict)
- **Choice:** in the agent-v3 diagnostic runs (dev, toy-discount, graph off), reasoning on resolved 5/5 and off 4/5. Scratch files written into the repo showed up in the final diff, and agents added their own test files, which the regression command then ran. v4 fixes the bookkeeping around that.
- **Rejected:** (a) Deleting scratch files from the repo before verification: the model can't know which of its new files the harness would keep. A separate directory makes the boundary explicit.

### 035 · BullMQ queues (detector bullmq-queues@0.1), Worker files as entry points, ignore/entryPoints config; analyzer 0.7.0
- **Date:** 2026-10-01 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#035--bullmq-queues-detector-bullmq-queues01-worker-files-as-entry-points-ignoreentrypoints-config-analyzer-070)
- **Choice:** **Facts** (`src/extract/bullmq.ts`, `FileFacts.queueOps`, present only when non-empty so snapshots of repos without BullMQ are unchanged): - `new Queue(name, …)` → a `define` fact (the variable, and the Redis connection when the `connection` option resolves to a known redis client).
- **Rejected:** (a) A new edge kind (`enqueues`/`processes`): produces/consumes through a broker already says it, and impact would need new direction rules for no new information.

### 036 · Docker is the sandbox of record
- **Date:** 2026-10-01 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#036--docker-is-the-sandbox-of-record)
- **Choice:** **Why:** Nebius support (case **AISTUDIOSUP-1966**) replied that Token Factory Sandboxes are not yet ready to be used on this account. Since 2026-09-30 the beta request had left `/whoami` with every permission `false` and list/spawn returning 403 (FEEDBACK.md). There is no date to plan around, so th
- **Rejected:** (a) Waiting for Sandboxes: no date, and the evaluation needs a sandbox now. (b) gVisor/Firecracker for a stronger boundary: not available on the macOS dev machine or by default on GitHub runners, and…

### 037 · A sandbox that expires or disappears is a harness failure with a named reason
- **Date:** 2026-10-01 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#037--a-sandbox-that-expires-or-disappears-is-a-harness-failure-with-a-named-reason)
- **Choice:** **Detection:** `LocalDockerProvider.status()` (`docker inspect --format {{.State.Running}}` → running / stopped / gone / unknown).
- **Rejected:** (a) Retrying in a fresh sandbox: the agent's state is gone, and a retry hides an infrastructure problem the record should show.

### 038 · Change sets: a declaration-level diff with an architectural rollup (analyzer 0.8.0)
- **Date:** 2026-10-02 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#038--change-sets-a-declaration-level-diff-with-an-architectural-rollup-analyzer-080)
- **Choice:** **Goal:** for any diff (two commits, or a repair run's patch), one machine-readable answer to: which components changed, which declarations changed and how, which call, route and queue relationships were added or removed, and what could break. Deterministic, from ts-morph and git only: no model is i
- **Rejected:** (a) A text diff mapped to files only: it can't say which function changed or which calls appeared. (b) Rename detection by body similarity: a heuristic that can be wrong silently;

### 039 · Change sets v2, a multi-repo snapshot index, and a version guard (analyzer 0.9.0)
- **Date:** 2026-10-02 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#039--change-sets-v2-a-multi-repo-snapshot-index-and-a-version-guard-analyzer-090)
- **Choice:** **Step 1 · Why so many calls were "dynamic" (cause proven before any change).** Recall @ `5d2165a`, all 61 files, counted with the 0.8.0 rules: 270 resolved, 844 external, **358 dynamic**. A diagnostic that records why each call was dynamic: - 138 had no symbol on the property, and the receiver was 

### 040 · Change view and repo gallery (viewer; change sets from the existing CLI)
- **Date:** 2026-10-02 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#040--change-view-and-repo-gallery-viewer-change-sets-from-the-existing-cli)
- **Choice:** **Goal:** a reviewer understands what a diff did to the system without reading its lines: component level first, then files and declarations, with "what could break" (the change set's warnings) first. Viewer only: change sets come from `tracehound changes` unmodified; no model. **Data (`changesets/`
- **Rejected:** (a) Redrawing component edges from declaration edges in the viewer: it would draw edges the CLI didn't (product rule); the rollup's `componentEdges` are used as they are.

### 041 · Repo profiles, a baseline-matched typecheck gate, and the repair harness on Recall
- **Date:** 2026-10-02 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#041--repo-profiles-a-baseline-matched-typecheck-gate-and-the-repair-harness-on-recall)
- **Choice:** **Repo profile** (`configs/<repo>.profile.json`, `src/harness/profile.ts`, zod, strict): ``` RepoProfile { id: string // "recall" workdir: string = "." // repo-relative; every profile command runs here; no "..", not absolute install?: string // run once with the network on;
- **Rejected:** (a) Seeding with `sed` in a setup command: no file to review, and a no-op substitution exits 0. (b) A seed commit on a Recall branch: Recall is not to be changed, and the agent could find it.

### 042 · BullMQ queues through dependency injection (detector bullmq-queues@0.2; analyzer 0.10.0)
- **Date:** 2026-10-03 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#042--bullmq-queues-through-dependency-injection-detector-bullmq-queues02-analyzer-0100)
- **Choice:** decision 041 found that at Recall `57d920e` the API → queue `produces` edge is gone: the producer calls `.add` on a value destructured from a function parameter typed as a hand-written interface, and the real `Queue` is built in a factory and passed in at the function's call site.
- **Rejected:** (a) Following any number of hops: each hop multiplies the call sites that can disagree, and the edge gets harder to point to.

### 043 · A per-test regression gate; study S1 recorded
- **Date:** 2026-10-03 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#043--a-per-test-regression-gate-study-s1-recorded)
- **Choice:** **Repo profile** gains an optional `testReport: { format: "junit", command }`. The command is the suite again with the reporter on, and must contain `$REPORT`; it needs `test`. Recall: `bun test --reporter=junit --reporter-outfile=$REPORT`.
- **Rejected:** (a) Parsing bun's console output (`(pass)` / `(fail)` lines): it's a display format, and multi-line names and errors between tests make it ambiguous.

### 044 · Four seeded dev tasks on Recall
- **Date:** 2026-10-03 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#044--four-seeded-dev-tasks-on-recall)
- **Choice:** **Task kind `"dev"`** (`TaskSpec.kind`, copied to `taskKind`): a task the prompts may be tuned on. Like `smoke`, never an evaluation result.
- **Rejected:** (a) Seeds that make an existing test fail: the failing test points at the file, and the per-test gate would have one test fewer to watch.

### 045 · The graph-on arm: the context packet in the first message (agent-v5)
- **Date:** 2026-10-03 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#045--the-graph-on-arm-the-context-packet-in-the-first-message-agent-v5)
- **Choice:** until now "graph on" only added the graph tools and one prompt sentence; in the two graph-on smoke runs and the four graph-on baseline runs below the agent never called a graph tool.
- **Rejected:** (a) Injecting the packet as a fake tool result: it would cost a step in one arm only. (b) The JSON packet the `context_packet` tool returns: the CLI's text form is shorter and is what "the existing co…

### 046 · The evaluation runner
- **Date:** 2026-10-03 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#046--the-evaluation-runner)
- **Choice:** the evaluation needs one command that runs every task in both arms, with repeats, and writes the results (prompt `docs/prompts/build-to-freeze.md`, Phase 4). The held-out tasks live outside this repo.
- **Rejected:** (a) Calling `runRepair` in-process: one client and one per-process cap for all runs, and a crash in one run would end the others. (b) Parallel runs by default: see decision 045.

### 047 · agent-v6: the str_replace_editor alias and an edit retry for line endings
- **Date:** 2026-10-03 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#047--agent-v6-the-str_replace_editor-alias-and-an-edit-retry-for-line-endings)
- **Choice:** in the 32 agent-v5 dev runs (decisions 044–046) every run that called `finish` was RESOLVED and all 19 UNRESOLVED runs ended on a budget (18) or the stuck stop (1). The agent read the fault file in every run.
- **Rejected:** (a) Adding `str_replace_editor` to the schemas: a second edit tool both arms would have to be told about, i.e. a prompt change.

### 048 · The reproduce stage: a claim, one agent-written test, a check independent of the agent
- **Date:** 2026-10-04 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#048--the-reproduce-stage-a-claim-one-agent-written-test-a-check-independent-of-the-agent)
- **Choice:** the findings pipeline needs a stage before repair. Given a claim that a repo misbehaves, it has to decide whether the claim holds at the base commit, and it must not rely on the agent's word for it (prompt `docs/prompts/reproduce-stage.md`).
- **Rejected:** (a) Judging in the agent's own sandbox: the agent could leave state behind (files in `/tmp`, caches) that makes its test pass or fail there only. The flaky case shows why.

### 049 · Evaluation protocol, amendment 1: "verified at stop", run order, canary per pass
- **Date:** 2026-10-05 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#049--evaluation-protocol-amendment-1-verified-at-stop-run-order-canary-per-pass)
- **Choice:** Amendment 1, appended verbatim to `docs/eval/protocol.md` before any held-out run: - a second measure, "verified at stop", reported beside "resolved" for every arm and model; - five passes per batch, each running every task in both arms once;
- **Rejected:** (a) Changing the harness so that a budget stop with a passing patch counts as RESOLVED: that changes a frozen artifact after the tag.

### 050 · Held-out results imported; the graph did not help the repair agent on Recall
- **Date:** 2026-10-06 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#050--held-out-results-imported-the-graph-did-not-help-the-repair-agent-on-recall)
- **Choice:** the results come into the repo as `docs/eval/heldout-results.md`, with every table recomputed from the 181 run records (`docs/eval/heldout/recompute.mjs`) rather than copied from the handoff, and the batch summaries copied unchanged into `docs/eval/heldout/`. Only files holding ids, counts and metrics come in;
- **Rejected:** (a) Copying the handoff's tables as they were: the records are the source, and the repo wins. - (b) Redacting the one row in `b2-super/results.json` and committing the edited copy: a copy of an archiv…

### 051 · Finder phase 1: graph rules that emit hypotheses, no model
- **Date:** 2026-10-06 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#051--finder-phase-1-graph-rules-that-emit-hypotheses-no-model)
- **Choice:** the held-out evaluation (050) found that the graph did not help the repair agent. The graph's remaining job is choosing what to check.
- **Rejected:** (a) New extractors that put middleware, payload keys and fetch calls into the snapshot: that changes snapshot output (version bump, regenerated published snapshots) for facts only the finder reads, an…

### 052 · Finder phase 2a: hypotheses become reproduce claims, no model
- **Date:** 2026-10-06 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#052--finder-phase-2a-hypotheses-become-reproduce-claims-no-model)
- **Choice:** the finder (051) writes hypotheses; the reproduce stage (048) takes claims. Phase 2a joins them without a model, so that a later phase can run the reproduce stage on finder output (prompt `docs/prompts/052-finder-claims.md`). Nothing here runs the reproduce stage. **Choice** (`packages/analyzer/src/finder/claims.ts`;
- **Rejected:** (a) A sibling command that converts a saved report: it would have to trust the report's commit matches the checkout the claims will name. Possible later if reports need re-converting.

### 053 · Disclosure: who saw Recall's real bugs, and what finder results on Recall may claim
- **Date:** 2026-10-07 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#053--disclosure-who-saw-recalls-real-bugs-and-what-finder-results-on-recall-may-claim)
- **Choice:** Every finder, reproduce or repair result on Recall's real bugs is reported with a pointer to decision 053. - Any change to the finder's rules, the claim text or the reproduce prompt made after 2026-10-07 counts as made with knowledge of the bugs, whoever writes it.
- **Rejected:** (a) Leaving the disclosure outside the repo, where readers of the results cannot see it. - (b) Describing the bugs in the entry, which would end blindness for every later session that reads this repo.

### 054 · Review benchmark: golden comments, recorded tool reviews, and our own judge
- **Date:** 2026-10-14 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#054--review-benchmark-golden-comments-recorded-tool-reviews-and-our-own-judge)
- **Choice:** **Dataset:** `ai-code-review-evaluations/golden_comments` @ `3f2c8ab794b4ed71c099cbf0a7670555e90906dc`, `code_review_benchmarks/{sentry,cal_dot_com,grafana, keycloak,discourse}.json`: 5 repos × 10 pull requests, 137 golden comments, 97 of them Medium or higher (sentry 21, cal_dot_com 23, grafana 16, keycloak 18, discou…
- **Rejected:** (a) Trusting any vendor's table: they disagree, their judges aren't all stated, and we can't rerun them on our own reviewer's output.

### 055 · Diff-only baseline reviewer, review-diff-v1
- **Date:** 2026-10-15 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#055--diff-only-baseline-reviewer-review-diff-v1)
- **Choice:** **What it sees:** the pull request's title, its description (cut at 8,000 characters) and its unified diff, nothing else from the repo: no files outside the diff, no graph, no history.
- **Rejected:** (a) Whole-repo context in this version: that is what later versions are measured against. - (b) Asking the model for a severity and filtering on it: not in v1, so the baseline stays untuned.

### 056 · Verification pass on review findings, verify-v1
- **Date:** 2026-10-15 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#056--verification-pass-on-review-findings-verify-v1)
- **Choice:** **What the verifier sees** (`packages/analyzer/src/review/verify.ts`, `tracehound verify run|filter`): one call per finding with the pull request's title, its description (cut at 8,000 characters as in 055), the finding (path, line, body) and the diff of that finding's file, rendered with new-version line numbers exact…
- **Rejected:** (a) Asking the reviewer itself for a severity or confidence: it would change review-diff-v1 and mix two changes. - (b) Dropping findings that depend on code outside the diff: that is the next job's co…

### 057 · Repository context for the reviewer, review-ctx-v1
- **Date:** 2026-10-15 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#057--repository-context-for-the-reviewer-review-ctx-v1)
- **Choice:** **Repositories** (`scripts/bench/fetch-repos.ts`): a bare clone of each dev repo's greptile fork outside the repo, plus `refs/pull/<n>/head`.
- **Rejected:** (a) TypeScript-only lookups (compiler-resolved symbols): most dev pull requests are not TypeScript and would not count. - (b) Letting the model search the repository with tools: in 050 the repair agen…

### 058 · Context-aware verifier, verify-ctx-v1
- **Date:** 2026-10-15 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#058--context-aware-verifier-verify-ctx-v1)
- **Choice:** **Pre-registered rules** (written before any model call in this job and not changed after the first verifier call; committed in the freeze commit "verify-ctx-v1: pre-registered rules and frozen prompt (058)"): - Verdict rule: a finding is kept if and only if the verifier's valid reply says keep.
- **Rejected:** (a) Reusing 057's per-chunk block: references reached the model in only 12 of 48 chunks. - (b) Repeated-run voting: costs N reviewer runs; the agreement table gives a first, free signal.

