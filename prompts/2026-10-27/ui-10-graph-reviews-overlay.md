# ui-10: the graph becomes the review surface (PR lane, findings pinned to components) + the dropdown bug

Tool: **Claude Code, Opus 5.5, effort High**, on Sunny's **account A** (`claude`). Start it **after relay's 071b has finished** (or now, if account A has usage headroom). Folder: a new worktree **`~/Projects/TraceHound-ui`**. Paste everything below the line.

Setup (Sunny, in a plain terminal, once):
```
cd ~/Projects/TraceHound && git fetch origin
git worktree add ~/Projects/TraceHound-ui -b ui/graph-v2 origin/main
cd ~/Projects/TraceHound-ui && pnpm install --frozen-lockfile && claude
```
(If `~/Projects/TraceHound-ui` already exists from earlier UI work: `cd ~/Projects/TraceHound-ui && git status` must be clean; then `git fetch origin && git switch -c ui/graph-v2 origin/main`.)

---

You are the UI lane for TraceHound (Nebius × NVIDIA hackathon; the video is recorded on **28 Oct afternoon**, so this PR must be **open, green and reviewable by 28 Oct 12:00 IST**). TraceHound has two halves today: a component **graph** of a TypeScript repo (`/graph`, React Flow + ELK, static snapshots) and a **PR reviewer** whose reviews are shown on separate pages (`/reviews`). Your job: **make the graph the place where reviews are seen.** Use Claude Code's `frontend-design` skill for the visual work.

## Read first
- `CLAUDE.md` (Stack, Conventions, Gates). `viewer/` is a static Next.js export: no server, no API routes, no auth, no DB.
- `viewer/components/GraphCanvas.tsx`, `ComponentNode.tsx`, `Inspector.tsx`, `Viewer.tsx`, `TopBar.tsx`, `Rail.tsx`; `viewer/lib/graph.ts`, `layout.ts`, `positions.ts`, `expansion.ts`, `changes.ts`, `reviews.ts`, `github.ts`, `repos.ts`, `highlight.ts`.
- The existing change view (`?changes=`, decision 038) and impact view (`?impact=`): reuse their highlight styles and plumbing.
- The brain's `research/R15-ui-references.md` (UI references; effect-first finding titles; checks in the merge box).

## Tasks, in this order (one commit each, gates green before each commit)

### 1. Fix the dropdown bug (first; a test that fails before the fix)
Symptom (Sunny): clicking a node's dropdown (expand) resets the whole layout, and the menu opens in the wrong place. Cause, from reading `GraphCanvas.tsx`: `expanded`/`heights` are in the ELK layout effect's dependency list, so every toggle re-runs ELK for all nodes, and saved (dragged) positions are only restored when `initial && !expanded.size`. Fix: expanding/collapsing changes **only that node's height** (push nodes below it down if they'd overlap, nothing else moves); no ELK re-run and no `fitView` on toggle; dragged positions survive. Anchor the menu/expanded contents to the node itself. Add a test in `viewer/test/expand-nodes.test.tsx` that other nodes' positions are unchanged after a toggle.

### 2. Recall snapshot at its new head
Recall's head is `62eaca1` (Sunny deleted an orphan file). If `snapshots/index.json`'s recall `latest.sha` isn't `62eaca1…` and no open PR does it, regenerate per CLAUDE.md (temp clone, `node packages/analyzer/src/cli.ts --repo "$R" --config configs/recall.tracehound.json --out snapshots --cache-only`; if a cache miss aborts, STOP and report: naming needs a model call and Sunny's approval). Fix any viewer test that pins the old sha by pointing it at the manifest, not by hard-coding the new one.

### 3. PR lane on the graph
- A **PR lane** (a horizontal strip or rail section on `/graph`): the repo's pull requests, newest first, each with state, the TraceHound review status (reviewed / no findings / not reviewed) and a finding count.
- **Data, at runtime from GitHub's public API, reusing `lib/reviews.ts` / `lib/github.ts`**: the PR list, each PR's changed files (`GET /repos/{o}/{r}/pulls/{n}/files`), and TraceHound's review (its footer lists findings with path and line).
- **Repo mapping** (in `lib/repos.ts`, data only): `tracehound-demo` snapshot ↔ `SunnyBagal/tracehound-demo`; `recall` snapshot ↔ `SunnyBagal/recall-seeded` **behind a flag that stays off** (those PRs are blind until scoring; chat flips it after). Repos with no mapping show no lane.
- **Selecting a PR**: components containing a changed file get the change-view highlight; the rest dim; files the graph doesn't cover go in an "outside the graph" list. Selecting none returns to the plain graph.
- **Rate limits** (unauthenticated: 60 requests/hour per viewer): cache every response in `sessionStorage`; on 403/429 show a calm notice and fall back to a **static review snapshot** if present. Add `viewer/scripts/reviews-snapshot.mjs` that Sunny runs locally (uses `gh api`, writes `viewer/public/reviews/<owner>__<repo>.json` with PRs, files and TraceHound's review footers only, no other bots' comments) and make the lane prefer the live API, then the static file.

### 4. Findings pinned to the graph
- With a PR selected, each component with TraceHound findings gets a **badge** (count) and a **callout** anchored to the node: the finding's first line, effect-first (R15), file:line, "model-generated, not tested" (existing `MODEL_DISCLOSURE`).
- Draw a **PR node** (or a lane marker) with edges to every affected component, visually distinct from evidence edges (dashed, labelled "changed in #N"). Evidence edges keep their meaning; never draw an edge that isn't a changed file or a finding location.
- Clicking a badge opens the findings for that component in the Inspector, each linking to its GitHub comment. Reuse `ReviewFinding.tsx` read-only (another lane restyles it; import it, don't edit it).
- Findings whose path matches no component go under "outside the graph", never dropped.

### 5. The graph page's look
Restyle the graph shell to feel alive and intentional (frontend-design skill): `app/globals.css` design tokens, `TopBar`, `Rail`, `ComponentNode`, `EvidenceEdge`, `Inspector`, the PR lane, motion on selection and highlight (CSS only; respects `prefers-reduced-motion`). You own `app/globals.css`'s tokens; the pages lane (Gemini) uses them read-only, so define them early (commit 5a: tokens; 5b: components) and keep their names stable.

## Rules
- **You own:** `viewer/app/graph/**`, `viewer/app/globals.css`, `viewer/app/layout.tsx`, `viewer/components/{GraphCanvas,ComponentNode,EvidenceEdge,Inspector,Viewer,TopBar,Rail,KindLegend,WarningsControl,RepoSwitcher,ImpactPanel,Change*}.tsx`, new graph components, `viewer/lib/**` (except `benchmark.ts`), `viewer/scripts/reviews-snapshot.mjs`, `viewer/public/reviews/`, `snapshots/`, their tests.
- **Don't touch:** `viewer/app/reviews/**`, `viewer/app/benchmark/**`, `viewer/app/page.tsx`, `viewer/components/landing/**`, `ReviewsDashboard.tsx`, `ReviewFinding.tsx`, `ReviewShell.tsx`, `viewer/lib/benchmark.ts` (the Gemini lane's), and anything outside `viewer/` and `snapshots/` (`packages/`, `scripts/`, `docs/`, `README.md`, `.github/`, `config/`).
- **No new dependencies** (ask first: `@xyflow/react`, elkjs, lucide, Tailwind 4 are there).
- **Product rule:** every edge is backed by evidence. PR/finding edges are a separate, labelled overlay.
- No model calls, no Nebius key, no tokens in the client. The static snapshot script uses the local `gh` auth and writes only public data.
- Never show Cal.com (the reviews code already refuses it). Don't open `linea-seeded`, `recall-seeded`, `tracehound-seeds` contents (the mapping entry is a string; the flag stays off).
- Gates before each commit: the `GIT_*` env check (CLAUDE.md), then `pnpm --filter @tracehound/viewer test`, `pnpm --filter @tracehound/viewer typecheck`, `pnpm --filter @tracehound/viewer build` as separate commands. Timeouts-only failures: rerun with `--testTimeout 30000`.
- Push after each task. Open a **draft PR** after task 1, titled `ui-10: graph as the review surface (PR lane, pinned findings) + expand fix`; mark it ready when tasks 1–4 are done. Devin reviews it. Never merge.

## Report (final message)
What each task delivered (with screenshots' paths if you took any), what's not done, the gates' results, the PR number and head sha, and the exact command Sunny runs for the static review snapshot.
