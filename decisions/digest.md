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

<!-- Entries 059–075 were added by hand on 2026-11-11 (bunny, backfill) from main `cd5525a`, in the same format. Regenerate the whole file when convenient. -->

### 059 · Pooled candidates filtered by the context verifier, pool-verify-v1
- **Date:** 2026-10-16 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#059--pooled-candidates-filtered-by-the-context-verifier-pool-verify-v1)
- **Choice:** **Pre-registered rules** (committed in the freeze commit before any verifier call): the pool is every review-diff-v1 Super finding plus every review-ctx-v1 Super finding that agrees with none of them (058's rule: same path, lines within 3). verify-ctx-v1 filters it unchanged, and review-diff-v1 findings reuse their 058 verdicts.
- **Rejected:** (a) Running review-diff-v1 again N times and pooling: costs N reviewer runs per pull request. - (b) Re-verifying review-diff-v1's findings: 058's verdicts already exist from the same frozen verifier.

### 060 · Scorer checks: a second judge, bench-judge-x1, and augmented precision, validity-v1
- **Date:** 2026-10-16 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#060--scorer-checks-a-second-judge-bench-judge-x1-and-augmented-precision-validity-v1)
- **Choice:** **Pre-registered rules** (committed in the freeze commit "060: pre-registered scorer checks, judge-x1 and validity-v1 frozen"). Runs: the five recorded tools and the six TraceHound runs, all Super, re-judged by bench-judge-x1 (Qwen3-235B, same prompt), plus validity-v1 on the deduplicated v1 FPs.
- **Rejected:** Replacing bench-judge-v1 as the scorer of record: it would move every earlier number after the fact. - A human-labelled validity set: no time before the freeze.

### 061 · Pre-registered plan for the one Cal.com test-set run
- **Date:** 2026-10-17 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#061--pre-registered-plan-for-the-one-calcom-test-set-run)
- **Choice:** **Configuration under test:** pool-verify-v1, exactly as in 059, all stages on 3 Super, reasoning on, temperature 0. Run 1 is the headline; runs 2–3 measure spread with cache reads off; three predictions; cap $15. Amended before merge (061a): one restart for a crashed run; fetches before the freeze tag.
- **Rejected:** Re-running the CLIs for the repeats: they always read the response cache, so a repeat would return run 1's replies. - Judging the five tools in every run: their reviews are fixed. - Letting a crashed run resume in its own directory.

### 062 · Result: the Cal.com test-set run (decision 061, as amended by 061a)
- **Date:** 2026-10-18 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#062--result-the-calcom-test-set-run-decision-061-as-amended-by-061a)
- **Choice:** ran 061's plan in order from `review-freeze-1` (`6b4276e`). Cal.com run 1 (n = 23), bench-judge-v1: pool-verify-v1 P 56.0% / R 60.9% / F 58.3%; review-diff-v1 F 53.8%. Predictions (a) right, (b) wrong, (c) right. Spend $5.30959 against $15.
- **Rejected:** re-running dev run 3 under a fresh allowance: 061's cap order cuts it, and a cap stop is never restarted.

### 063 · Benchmark results page and its framing (docs only)
- **Date:** 2026-10-18 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#063--benchmark-results-page-and-its-framing-docs-only)
- **Choice:** one page, `docs/bench/results.md`, with counts copied from the result files: the Cal.com run 1 table under both judges, pool-verify-v1's rank in every run × judge cell, the dev rank and spread, 061's predictions scored, every loss and void run, limits and cost; a short README section.
- **Rejected:** a headline claim of beating Greptile. The run-to-run spread is larger than the margin.

### 064 · Vote across runs (dev only): a loss
- **Date:** 2026-10-19 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#064--vote-across-runs-dev-only-a-loss)
- **Choice:** pre-register the variants, clustering, a fallback and the adoption rule in `docs/bench/vote-plan.md` (`57f0b91`) before any model call; run dev runs 4–6 (cap $8.50); vote over runs 1, 2r, 4, 5, 6. vote-k3of5 passes v1 (+5.5 F) but fails x1 (50.4% vs 51.3%): no variant adopted. Spend $5.41.
- **Rejected:** adopting vote-k3of5 on the v1 result alone; changing k or the cap after seeing results.

### 065 · Evidence verifier + deterministic gate (dev only): a loss
- **Date:** 2026-10-19 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#065--evidence-verifier--deterministic-gate-dev-only-a-loss)
- **Choice:** verify-evidence-v1 (verify-ctx-v1's context, a new system prompt that must quote 1–3 exact lines) and the deterministic `gate`; plan `9bfb3eb` pushed first. Against 5-run means: A F 41.6% (−0.7), B F 38.3% (−4.0), C 0 findings. Neither adopted. Spend $2.65.
- **Rejected:** tuning the verify-evidence-v1 prompt after seeing scores; requiring a quote on a drop.

### 066 · Results wording fixes, all-50 table, levers tried (docs only)
- **Date:** 2026-10-20 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#066--results-wording-fixes-all-50-table-levers-tried-docs-only)
- **Choice:** the published claim becomes "statistically tied with Greptile and Cursor on 10 blind PRs, 4th of 6 across all 50"; "3rd of six" is no longer used; the cc-01 audit's five fixes go in as dated notes; an all-50 table under both judges with two declared rows; a levers table; the viewer's tables must equal results.md.
- **Rejected:** headlining whichever all-50 row is better; rewriting the old text of 061 or 063; "ahead of CodeRabbit" as a claim (both 95% intervals include 0).

### 067 · Nemotron 3.5 Super as the reviewer and verifier model (dev only): adopted on 2 runs
- **Date:** 2026-10-22 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#067--nemotron-35-super-as-the-reviewer-and-verifier-model-dev-only-adopted-on-2-runs)
- **Choice:** plan, rule, baseline and a 2-run fallback frozen in `docs/bench/dev-3.5-plan.md` before the first call; #55 routes the 3.5 id to us-north1; amendment 1 voided run 7 (35 × HTTP 404). Runs 7r and 8: v1 F 50.4%, x1 F 55.1%; all three conditions pass, so 3.5 Super VL becomes the Action's default. Spend $7.98.
- **Rejected:** raising `max_tokens` or changing the prompt to cut the JSON failures (a new variant); raising the cap to finish run 9.

### 068 · Nemotron 3.5 default with partial reviews on failed chunks
- **Date:** 2026-10-22 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#068--nemotron-35-default-with-partial-reviews-on-failed-chunks)
- **Choice:** default the Action's `model` input to `nvidia/Nemotron-3.5-Super-VL`, and keep partial results on failed reviewer chunks to match how 067 was measured; the footer records failed/total chunks; ReviewBench adapters report `status=ok partial=<failed>/<chunks>`. Fixes stay on 3 Super.
- **Rejected:** keeping all-or-nothing reviews (nothing posted on about 1 in 6 PRs); falling back to 3 Super on failure (unmeasured).

### 069 · The v2 blind Cal.com run (second look at the test set)
- **Date:** 2026-10-23 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#069--the-v2-blind-calcom-run-second-look-at-the-test-set)
- **Choice:** one run, `--set cal_dot_com --run 4` with 3.5 Super VL at every stage, gated by `TRACEHOUND_TEST_RUN=069`, cap $3, pre-registered (`b2ec214`) before the first call. Result: v1 P 63.2% / R 52.2% / F 57.1%, x1 F 58.5%, all three predictions right, $0.7228. 069b: PR-level paired bootstrap intervals; "ahead"/"behind" only when the 95% interval excludes 0.
- **Rejected:** a second v2 Cal.com run or any change based on this one; updating results.md, the README or the viewer in the same job; committing an all-50 script.

### 070 · recall-v3 for ReviewBench: built, dev-selected, neither arm qualifies
- **Date:** 2026-10-24 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#070--recall-v3-for-reviewbench-built-dev-selected-neither-arm-qualifies-2026-10-24)
- **Choice:** build `recall-v3` (P009) as an additive pipeline and run it once on dev per arm against P009's **precision** floors: S (3 Super) P 14.0 < 20.0 (F 21.8), V (3.5 Super VL) P 20.9 < 38.0 (F 31.4). Neither qualifies; v3 stops, and the P008 winner gets the final. Token Factory honours no reasoning token budget.
- **Rejected:** tuning the lenses or the cap after seeing arm S; using `reasoning_effort: "low"` as the "budget".

### 071 · Nemotron 3 Ultra on dev via the free NVIDIA API: report only
- **Date:** 2026-10-24 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#071--nemotron-3-ultra-on-dev-via-the-free-nvidia-api-report-only-2026-10-24)
- **Choice:** an opt-in second provider (`TRACEHOUND_NVIDIA_API=1` and the exact Ultra id only; 300 s timeout; ledgered at $0). Dev runs 10–11: pool-verify-v1 v1 F 42.3 and 51.9, mean 47.1, no interval-backed difference from 3 Super or 3.5. Nothing adopted; the Action stays on 3.5.
- **Rejected:** raising the frozen `max_tokens` or the 300 s timeout after the smoke; Ultra on Token Factory (unaffordable).

### 072 · review-freeze-2.1: the Action's install step only
- **Date:** 2026-10-27 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#072--review-freeze-21-the-actions-install-step-only-2026-10-27)
- **Choice:** `review-freeze-2.1` = `review-freeze-2` plus one commit (`86fd5d7`) that changes only the composite action's install step (setup-node, then a global pnpm install in the action's own checkout), so the reviewed repo's `packageManager` pin is never read. An install matrix of six root `package.json` cases fails for pnpm@9 before and passes all six after (#68).
- **Rejected:** keeping `pnpm/action-setup` with `package_json_file`: its default path resolves in the workspace.

### 073 · Pre-registered dev A/B of the two generation-recovery switches
- **Date:** 2026-11-08 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#073--pre-registered-dev-ab-of-the-two-generation-recovery-switches-2026-11-08)
- **Choice:** arms A (both off), B1 (lenient parser), B2 (lenient + fresh retry) on the 40 dev PRs over one replay corpus; exit criteria E2 / E3 (devin's gate) / E4; cap $4. 073a: first run void (Node's 300 s fetch cutoff), one restart, product transport fix, failure audit. Result: B1 passes, B2 fails (7 failed chunks); B1 relabelled "harmless, benefit unproven"; lenient parser default; standing rule: reliability tests bypass the cache. Spend $0.73195.
- **Rejected:** the research run's E3 (ΔTP ≥ 0 ∧ ΔFP ≤ 0); three independent fresh captures (over the cap, unpaired); a lenient verifier.

### 074 · Pre-registered: recovering empty replies on the hard-chunk set
- **Date:** 2026-11-08 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#074--pre-registered-recovering-empty-replies-on-the-hard-chunk-set-2026-11-08)
- **Choice:** the 14 distinct reviewer chunk requests behind runs 7r/8/9's 20 failures, 3 fresh draws each (42 units), cache bypassed; arms A (append retry), D (split in half), E (reasoning off + JSON schema retry); arm C (reasoning budget) dropped as unsupported. Result: E 41 of 42 successes vs A 22 meets the gate; D does not. $3.36463. Nothing adopted.
- **Rejected:** arm C via `reasoning_effort` (a level, not a budget); drawing each of the 20 failures; separate first draws per arm.

### 075 · Pre-registered: verified quality of 074's A vs E findings
- **Date:** 2026-11-08 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#075--pre-registered-verified-quality-of-074s-a-vs-e-findings-2026-11-08)
- **Choice:** verify-ctx-v1 unchanged on each arm's findings, one judge (bench-judge-v1) on the dev goldens, gate P(E) ≥ P(A) and TP(E) − TP(A) ≥ 3, claim wording fixed in advance, cap $1.50. Result: INCONCLUSIVE (one judge HTTP 529), $0.49090; E fails the gate under every completion. Closed 2026-11-09: E dropped, defaults unchanged, empty replies a known limitation.
- **Rejected:** not stated as a separate list in the entry.

### 076 · S0 rulings and the pre-registered plan to the held-out run
- **Date:** 2026-11-11 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076--s0-rulings-and-the-pre-registered-plan-to-the-held-out-run-2026-11-11)
- **Choice:** the plan runs only as corrected by its audit ([audit note](../project/plan-2026-11-10-audit.md)): job caps fixed ($18.31 usable; the paid context step cut), per-batch run caps, re-plan above a console ÷ ledger ratio of 1.08; ties keep the incumbent; the arm-selection, redraw and non-regression gates as the plan wrote them, each with its estimated error range; one defect fixed in several files counts as one bug, primary chosen by git path order; headline metric = bugs caught of 20 with a Wilson interval, comments and cost per PR; a 72 h window with the same PR order for every tool; the firewall lifts only after held-out outputs and scores are frozen and hashed. A check after recording found the caps can still stop the two dev runs and the held-out run early under a realistic cost spread; options are with Sunny.
- **Rejected:** running the plan as written; "the simpler arm wins" ties; the paid context step at ≈ $2.17; choosing a multi-file primary by hand.

### 076a · Caps after the caps check, and the protocol's open choices
- **Date:** 2026-11-11 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076a--caps-after-the-caps-check-and-the-protocols-open-choices-2026-11-11)
- **Choice:** every runner job's batch cap is the job cap minus what the job has spent, with unstarted cases rolled into the next batch; dev run 1's cap rises to $4.68 (unallocated $1.18); the dev run 2 and held-out caps are recomputed once from dev run 1's measured costs (95% completion) before dev run 2 starts, held-out first; an unstarted held-out case runs from the reserve; judge-only jobs get explicit caps. Protocol: α 0.05 two-sided; one rerun of a failed case; a tool with no review after one re-trigger 24 h later leaves that case out of its paired comparison; the hand-check sheet hides the judges' verdicts; sheet hashed before unblinding, score files hashed before the firewall lifts; the claim names every repo with a held-out case. Three newer protocol proposals wait for a planner review.
- **Rejected:** the first batch rule; fixing the later caps before any TypeScript cost is measured.

### 076b · R1/R2 judging envelope, re-trigger slots, CodeRabbit allowance, canary
- **Date:** 2026-11-11 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076b--r1r2-judging-envelope-re-trigger-slots-coderabbit-allowance-canary-2026-11-11)
- **Choice:** the dev runs' post-run judging stays inside each run's cap (one call at a time); in the held-out window first triggers go before re-triggers, and a re-trigger takes the first free hourly slot at least 24 h after its PR's own trigger, in PR order; CodeRabbit's hourly rate is recorded at the start and end of the dev PR step, and if it shrank the held-out window starts at least 7 days after that step's last review; a canary is permitted only after two consecutive held-out provider or transport failures, with at most 3 tries at least 30 minutes apart and at most $0.40 in total from the reserve, and if all 3 fail the run stops and Sunny is told; an adopted alternative arm is reported with the cost actually paid per PR and its own calls beside it.
- **Rejected:** judging under the $1 per-process default; re-triggers interleaved with first triggers; an open-ended canary.

### 076c · Planner B's protocol review, premortem and team process ruled
- **Date:** 2026-11-12 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076c--planner-bs-protocol-review-premortem-and-team-process-ruled-2026-11-12)
- **Choice:** the held-out protocol's three pending proposals adopted as a planner worded them (the one case whose labelled lines could not be traced stays in the 20, with a "of 19 without it" line for every tool; a target's range in a file is the span of the labellers' overlapping ranges; each paid job's starting ledger value is written to a file, one paid process runs at a time, and the redraw test gets a three-way verdict that counts missing draws). A review counts as completed when the tool's bot posted any review, inline comment or issue comment after the trigger (not a rate-limit or error message); a PR is re-triggered only if it has no completed review 24 h after its trigger. The registered tests are TraceHound against each of the two tools, two-sided at 0.05, uncorrected, with exact p-values; a pair with fewer than 15 of 20 bug cases reviewed by both is reported as counts only. The hand-check sheet gets a "which tool wrote this" guess column. The hourly triggers run from a detached scheduler. From the window's start, the tuning agents make no read of the forks' pull requests, issues or notifications. All ten proposed team-process changes are accepted.
- **Rejected:** dropping or replacing the untraceable case; one labeller's range alone; re-triggering completed reviews that had no comments; a correction across the two registered tests.

### 076d · Ancestry exposure, not-completion messages, scheduler lane, untriggered cases
- **Date:** 2026-11-13 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076d--ancestry-exposure-not-completion-messages-scheduler-lane-untriggered-cases-2026-11-13)
- **Choice:** a count found that 11 of the 20 held-out bug fixes are reachable from another case's branch on the same fork (5 of 29 held-out bases and 7 of 40 dev bases carry another case's fix; one of the four repos has none). The run goes ahead with no case excluded: every dev case branch is deleted from the forks after the dev PR step, held-out branches are pushed one at a time in base-date order, each only at its own first trigger, the report gives each tool's catches on exposed and unexposed bug cases separately, and the write-up states what remains as a limitation. If exposure at first review cannot reach 0, the remaining count goes back to Sunny before the freeze. Bot messages such as "in progress", "review skipped" or "paused" are not a completed review. The window scheduler moves to the main builder, with code review and the 8-slot rehearsal re-run on it. The collector needs an explicit flag for cases that were never triggered, and such a collection never drives a re-trigger. Addendum: a failed first trigger is retried before any later branch is pushed, two failed tries mark the case as failed, and exposure is judged per review at its actual time from the scheduler's log of the fork's branches. A later correction: the 11-of-20 and 5-of-29 counts are different sets, not one exposure counted two ways; exposure at first review under the push rules is 0 of 20 (two independent counts). Also recorded: the 9 Nov console balance was $19.13, not $19.31 (a typo), so $18.13 is usable. The spend freeze was lifted on 13 Nov as Sunny's decision without the console check; the remaining guard is the smoke test's console-to-ledger ratio (stop above 1.08).
- **Rejected:** excluding the exposed cases now (only if the re-count is above 0); rebuilding the case branches; an implicit window-start fallback in the collector.

### 076e · Planner A's 076d open items ruled; window dates
- **Date:** 2026-11-13 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076e--planner-as-076d-open-items-ruled-window-dates-2026-11-13)
- **Choice:** the smoke test runs through the window scheduler for real, so its first real run comes before the window (one extra CodeRabbit review accepted); 6 spare hourly slots, fixed command timeouts, held-out branches pushed in order of the base commit's committer date; T0 is when the first held-out PR opens, the snapshot 72 h later; one exposed set for all tools (the union over the two tools' reviews); dev branches deleted by a reviewed script, with leftover pull-request refs counted and stated as a limitation; window option B (a scheduled start two days later than option A). Addenda: the deletion lives in its own script; the smoke test starts from a tools file with both tools on and re-triggers off; a 78-hour stay-awake with a preflight check; agents other than the held-out reviewer run git in the test-set repo only with explicit paths (two incidents logged as counts: 8 file names shown each, 0 files opened); a 15-minute stop line with a 900-second pre-freeze check, and the smoke run must end with both slots done.
- **Rejected:** per-tool exposed sets; hand-typed deletes; option A.

### 076f · Dev table target, P1's start, interim table, unnamed bot messages, Mac
- **Date:** 2026-11-13 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076f--dev-table-target-p1s-start-interim-table-unnamed-bot-messages-mac-2026-11-13)
- **Choice:** the dev table compares TraceHound's three arms with CodeRabbit on the 40 dev cases, labelled one draw, one judge, not hand-checked; the dev PR step's $0 parts start as soon as the smoke test ends, without waiting for the billing check, while the first paid dev run still waits for it (ratio at most 1.08); an interim table only inside the $0.40 arm-scoring cap; any unnamed kind of bot message goes to Sunny before the patterns freeze.
- **Rejected:** Greptile on dev (reversed by 076g); waiting for the ratio before $0 steps.

### 076g · Fable's assessment adopted: A0 frozen, Greptile on dev, Claude rows, pooled test, stop/go rule
- **Date:** 2026-11-13 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076g--fables-assessment-adopted-a0-frozen-greptile-on-dev-claude-rows-pooled-test-stopgo-rule-2026-11-13)
- **Choice:** goals are a viability read from the dev table by 15–17 Nov, a credible held-out result and low spend. The Nemotron reviewer (A0) is frozen as the registered primary; the arm comparison is report-only; the second dev run, the redraw test, the union test and the interim table are cut, returning $5.05 to unallocated ($6.05). Greptile also reviews the 38 dev cases (100 credits − 2 − 38 = 60 left for held-out, once the expiry date is read). Report-only Claude rows on dev at Sunny's own cost (Haiku 5.5 on 40 cases, cap $4; Sonnet 5.5 on 10, cap $8), same prompts, any differing setting recorded; Nemotron stays primary. A secondary pooled analysis (31 + 20 = 51 bugs) is pre-registered before any dev number. Stop/go on the 31 dev bugs: go if A0's catches are at least CodeRabbit's minus 3 with at most twice its comments per PR; rethink if CodeRabbit leads by 6 or more; otherwise wait for held-out. The billing ratio is checked after the first paid batch (stop above 1.08). The orchestrator decides operator detail; Sunny rules on goals, money and test design. Freeze target 16 Nov; window no earlier than 17 Nov.
- **Rejected:** a further Nemotron run for the comparison; tuning prompts for Claude models; Greptile kept off dev.

### 076h · Apps on the forks since 14 Nov, P1's hold lifted, 3660 s spacing
- **Date:** 2026-11-14 · [repo entry](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#076h--apps-on-the-forks-since-14-nov-p1s-hold-lifted-3660-s-spacing-2026-11-14)
- **Choice:** the two review apps had been installed on the TraceHound repo only, not on the test forks, until the early hours of 14 Nov, so the smoke test's first triggers got no bot activity; after the fix, a re-trigger got both tools reviewing both smoke-test PRs (4 of 4 reviews, each finished in 2–4 minutes; 0 inline comments and 0 review objects, each ending with a summary comment and a check run). CodeRabbit allows 1 review per hour. The dev PR step's hold is lifted; trigger slots and spacing become 3660 seconds, counted from the end of the previous command; the collection close stays fixed 24 h after the last first trigger. A code check then found that findings are counted from inline comments only (an earlier ruling), so summary-only reviews score 0 catches; whether to count summary findings is open for Sunny.
- **Rejected:** a re-trigger extending the collection close.
