# Decision index

Every decision in the repo's `docs/decisions.md` (the source of truth), plus the vault's own notes. For a one-paragraph summary of each decision, see the [digest](digest.md). A vault note exists only where we judged the decision afterwards. Process decisions made in chat (P-numbers) are not in the repo.

## Repo decisions

| # | Decision | Vault note |
|---|---|---|
| 001 | [Run TypeScript with Node's native type stripping](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#001--run-typescript-with-nodes-native-type-stripping) |  |
| 002 | [TypeScript 6.x for type-checking, not 7.x](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#002--typescript-6x-for-type-checking-not-7x) |  |
| 003 | [Group by anchor reachability, not by directory or community detection](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#003--group-by-anchor-reachability-not-by-directory-or-community-detection) |  |
| 004 | [One Redis node per connection, keys listed on the node](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#004--one-redis-node-per-connection-keys-listed-on-the-node) |  |
| 005 | [Queue edges follow data flow; everything else follows dependency](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#005--queue-edges-follow-data-flow-everything-else-follows-dependency) |  |
| 006 | [Embed code snippets in the snapshot](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#006--embed-code-snippets-in-the-snapshot) |  |
| 007 | [Resolve static values with decaying confidence](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#007--resolve-static-values-with-decaying-confidence) |  |
| 008 | [Client-only modules fold into their resource node](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#008--client-only-modules-fold-into-their-resource-node) |  |
| 009 | [Static snapshot files + manifest instead of an API (hackathon scope)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#009--static-snapshot-files--manifest-instead-of-an-api-hackathon-scope) |  |
| 010 | [tracehound.json overrides pin files and then act like anchors](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#010--tracehoundjson-overrides-pin-files-and-then-act-like-anchors) |  |
| 011 | [Categorical confidence labels on edges; numbers stay internal](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#011--categorical-confidence-labels-on-edges-numbers-stay-internal) |  |
| 012 | [LLM naming is a separate, facts-only, fail-safe pass](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#012--llm-naming-is-a-separate-facts-only-fail-safe-pass) |  |
| 013 | [Hard budget caps enforced before each call, priced pessimistically](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#013--hard-budget-caps-enforced-before-each-call-priced-pessimistically) |  |
| 014 | [Content-addressed response cache](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#014--content-addressed-response-cache) |  |
| 015 | [Left-to-right layered layout (ELK `layered`, direction RIGHT)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#015--left-to-right-layered-layout-elk-layered-direction-right) |  |
| 016 | [Floating edges instead of fixed left/right handles](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#016--floating-edges-instead-of-fixed-leftright-handles) |  |
| 017 | [Confidence is a dash pattern plus text, never style alone](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#017--confidence-is-a-dash-pattern-plus-text-never-style-alone) |  |
| 018 | [Viewer serves copied snapshots; the repo's `snapshots/` stays the source of truth](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#018--viewer-serves-copied-snapshots-the-repos-snapshots-stays-the-source-of-truth) |  |
| 019 | [Vercel Root Directory = `viewer`, with files outside the root included in the build](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#019--vercel-root-directory--viewer-with-files-outside-the-root-included-in-the-build) |  |
| 020 | [Naming runs with reasoning off (`chat_template_kwargs.enable_thinking=false`)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#020--naming-runs-with-reasoning-off-chat_template_kwargsenable_thinkingfalse) |  |
| 021 | [Deterministic checks on model names before they're stored](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#021--deterministic-checks-on-model-names-before-theyre-stored) |  |
| 022 | [Drive Nebius Sandboxes from TypeScript over REST, no Python bridge](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#022--drive-nebius-sandboxes-from-typescript-over-rest-no-python-bridge) |  |
| 023 | [Impact analysis: which way to walk each edge kind](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#023--impact-analysis-which-way-to-walk-each-edge-kind) |  |
| 024 | [Crossing a queue costs one hop of impact depth](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#024--crossing-a-queue-costs-one-hop-of-impact-depth) |  |
| 025 | [Agent context packets: deterministic lexical ranking v1, heuristic confidence](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#025--agent-context-packets-deterministic-lexical-ranking-v1-heuristic-confidence) |  |
| 026 | [Repair harness core: provider interface, harness-owned verification, Docker first](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#026--repair-harness-core-provider-interface-harness-owned-verification-docker-first) |  |
| 027 | [Decider interface; Nemotron localizer over deterministic facts, with a visible fallback](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#027--decider-interface-nemotron-localizer-over-deterministic-facts-with-a-visible-fallback) |  |
| 028 | [Viewer: fact-backed tech icons, overlay inspector with history-backed navigation](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#028--viewer-fact-backed-tech-icons-overlay-inspector-with-history-backed-navigation) |  |
| 029 | [Repair agent loop: host-side TypeScript, native tool calls](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#029--repair-agent-loop-host-side-typescript-native-tool-calls) |  |
| 030 | [Base agent competence: agent-v2 (same in both graph conditions)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#030--base-agent-competence-agent-v2-same-in-both-graph-conditions) |  |
| 031 | [Viewer: capped zoom, a header icon rule, transient panel↔canvas highlight, left rail](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#031--viewer-capped-zoom-a-header-icon-rule-transient-panelcanvas-highlight-left-rail) |  |
| 032 | [Git env hygiene: every git spawn drops the repo-selecting GIT_* variables](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#032--git-env-hygiene-every-git-spawn-drops-the-repo-selecting-git_-variables) |  |
| 033 | [agent-v3: output cap, stuck stop, one no-edit nudge, one way to run scratch code](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#033--agent-v3-output-cap-stuck-stop-one-no-edit-nudge-one-way-to-run-scratch-code) |  |
| 034 | [agent-v4: reasoning on by default, /scratch outside the repo, base-file edit accounting, agent tests out of the verdict](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#034--agent-v4-reasoning-on-by-default-scratch-outside-the-repo-base-file-edit-accounting-agent-tests-out-of-the-verdict) |  |
| 035 | [BullMQ queues (detector bullmq-queues@0.1), Worker files as entry points, ignore/entryPoints config; analyzer 0.7.0](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#035--bullmq-queues-detector-bullmq-queues01-worker-files-as-entry-points-ignoreentrypoints-config-analyzer-070) |  |
| 036 | [Docker is the sandbox of record](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#036--docker-is-the-sandbox-of-record) |  |
| 037 | [A sandbox that expires or disappears is a harness failure with a named reason](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#037--a-sandbox-that-expires-or-disappears-is-a-harness-failure-with-a-named-reason) |  |
| 038 | [Change sets: a declaration-level diff with an architectural rollup (analyzer 0.8.0)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#038--change-sets-a-declaration-level-diff-with-an-architectural-rollup-analyzer-080) |  |
| 039 | [Change sets v2, a multi-repo snapshot index, and a version guard (analyzer 0.9.0)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#039--change-sets-v2-a-multi-repo-snapshot-index-and-a-version-guard-analyzer-090) |  |
| 040 | [Change view and repo gallery (viewer; change sets from the existing CLI)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#040--change-view-and-repo-gallery-viewer-change-sets-from-the-existing-cli) |  |
| 041 | [Repo profiles, a baseline-matched typecheck gate, and the repair harness on Recall](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#041--repo-profiles-a-baseline-matched-typecheck-gate-and-the-repair-harness-on-recall) |  |
| 042 | [BullMQ queues through dependency injection (detector bullmq-queues@0.2; analyzer 0.10.0)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#042--bullmq-queues-through-dependency-injection-detector-bullmq-queues02-analyzer-0100) |  |
| 043 | [A per-test regression gate; study S1 recorded](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#043--a-per-test-regression-gate-study-s1-recorded) |  |
| 044 | [Four seeded dev tasks on Recall](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#044--four-seeded-dev-tasks-on-recall) |  |
| 045 | [The graph-on arm: the context packet in the first message (agent-v5)](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#045--the-graph-on-arm-the-context-packet-in-the-first-message-agent-v5) |  |
| 046 | [The evaluation runner](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#046--the-evaluation-runner) |  |
| 047 | [agent-v6: the str_replace_editor alias and an edit retry for line endings](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#047--agent-v6-the-str_replace_editor-alias-and-an-edit-retry-for-line-endings) |  |
| 048 | [The reproduce stage: a claim, one agent-written test, a check independent of the agent](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#048--the-reproduce-stage-a-claim-one-agent-written-test-a-check-independent-of-the-agent) |  |
| 049 | [Evaluation protocol, amendment 1: "verified at stop", run order, canary per pass](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#049--evaluation-protocol-amendment-1-verified-at-stop-run-order-canary-per-pass) |  |
| 050 | [Held-out results imported; the graph did not help the repair agent on Recall](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#050--held-out-results-imported-the-graph-did-not-help-the-repair-agent-on-recall) | [D050-graph-did-not-help](D050-graph-did-not-help.md) |
| 051 | [Finder phase 1: graph rules that emit hypotheses, no model](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#051--finder-phase-1-graph-rules-that-emit-hypotheses-no-model) |  |
| 052 | [Finder phase 2a: hypotheses become reproduce claims, no model](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#052--finder-phase-2a-hypotheses-become-reproduce-claims-no-model) |  |
| 053 | [Disclosure: who saw Recall's real bugs, and what finder results on Recall may claim](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#053--disclosure-who-saw-recalls-real-bugs-and-what-finder-results-on-recall-may-claim) | [D053-recall-disclosure](D053-recall-disclosure.md) |
| 054 | [Review benchmark: golden comments, recorded tool reviews, and our own judge](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#054--review-benchmark-golden-comments-recorded-tool-reviews-and-our-own-judge) | [D054-review-benchmark-scorer](D054-review-benchmark-scorer.md) |
| 055 | [Diff-only baseline reviewer, review-diff-v1](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#055--diff-only-baseline-reviewer-review-diff-v1) | [D055-diff-only-baseline](D055-diff-only-baseline.md) |
| 056 | [Verification pass on review findings, verify-v1](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#056--verification-pass-on-review-findings-verify-v1) | [D056-verify-v1](D056-verify-v1.md) |
| 057 | [Repository context for the reviewer, review-ctx-v1](https://github.com/SunnyBagal/TraceHound/blob/main/docs/decisions.md#057--repository-context-for-the-reviewer-review-ctx-v1) | [D057-repo-context](D057-repo-context.md) |
| 058 | Context-aware verifier, verify-ctx-v1 (PR #27) | [D058-verify-ctx](D058-verify-ctx.md) |
| 059 | Pooled candidates filtered by the context verifier, pool-verify-v1 (adopted, PR #29) | [D059-pool-verify](D059-pool-verify.md) |
| 060 | Scorer checks: second judge Qwen3-235B and augmented precision (brief written) | [D060-scorer-checks](D060-scorer-checks.md) |

## Process decisions (chat, not in the repo)

| # | Decision | Note |
|---|---|---|
| P001 | TraceHound becomes a PR reviewer measured on a public benchmark (2026-10-07) | [P001-pivot-to-reviewer](P001-pivot-to-reviewer.md) |
| P002 | Lanes: relay for the benchmark, Codex for the product, Grok for audits (2026-10-14/15) | [P002-lanes-and-tools](P002-lanes-and-tools.md) |
| P003 | Grok kill test: every audit finding must cite a real file and line | [P003-grok-kill-test](P003-grok-kill-test.md) |
| P004 | Pitch the honest benchmark and the live product, not "beats Greptile" (proposed 2026-10-15) | [P004-target-reset](P004-target-reset.md) |
