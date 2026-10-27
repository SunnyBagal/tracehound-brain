> **SUPERSEDED (27 Oct 17:45): replaced by ui-11. Don't run.**

# gemini-02: restyle the landing, reviews and benchmark pages (no data changes)

Tool: **Gemini CLI** (Gemini Pro), default effort is fine. Folder: a new worktree **`~/Projects/TraceHound-ui2`**. Can start **now**, in parallel with ui-10.

Setup (Sunny, in a plain terminal, once):
```
cd ~/Projects/TraceHound && git fetch origin
git worktree add ~/Projects/TraceHound-ui2 -b ui/pages-v2 origin/main
cd ~/Projects/TraceHound-ui2 && pnpm install --frozen-lockfile && gemini
```
Then paste everything below the line.

---

You're redesigning three pages of TraceHound's static Next.js site (`viewer/`): the landing page (`/`), the reviews pages (`/reviews`, `/reviews/pr`), and the benchmark page (`/benchmark`). The owner's verdict on today's versions: "dull, boring, looks AI-generated". Make them feel designed by a person with taste: a clear visual idea, real hierarchy, purposeful motion, nothing generic. Deadline: a PR open and green by **28 Oct 12:00 IST** (the demo video is recorded that afternoon).

## Read first
- `CLAUDE.md` (Stack and Gates sections). Static export: no server, no API routes.
- `viewer/app/page.tsx`, `viewer/components/landing/**`, `viewer/app/reviews/**`, `viewer/components/{ReviewsDashboard,ReviewFinding,ReviewShell}.tsx`, `viewer/app/benchmark/page.tsx`, `viewer/lib/benchmark.ts`, `viewer/test/benchmark.test.tsx`, `viewer/test/reviews-dashboard.test.tsx`, `viewer/test/review-finding.test.tsx`, `viewer/test/landing.test.tsx`.
- `viewer/app/globals.css`: **another lane (ui-10) owns the design tokens** there and is updating them today. Use its CSS variables; don't edit that file. Fetch `origin/ui/graph-v2` now and then (`git fetch origin && git show origin/ui/graph-v2:viewer/app/globals.css`) to stay consistent; if you need a token that doesn't exist, define it locally in your page's styles and list it in your report.

## What to do
1. **Benchmark page:** keep every number exactly as `viewer/lib/benchmark.ts` has it, **don't edit `benchmark.ts`**, because `benchmark.test.tsx` checks the tables equal `docs/bench/results.md`, and a separate job updates both later today. Redesign the presentation: lead with the one honest headline, show uncertainty (intervals, "point estimate", "statistically tied") visually rather than hiding it, make the losses section readable and proud, not buried. Tables stay real `<table>`s (accessibility, the test).
2. **Reviews pages:** the dashboard and a single PR's review. Make findings scannable: effect-first titles, file:line, the "model-generated, not tested" disclosure kept, clear states for loading, rate-limited, no findings, not reviewed.
3. **Landing page:** one strong idea of what TraceHound is (a repo's architecture graph + a PR reviewer on Nemotron via Nebius Token Factory), with links into `/graph`, `/reviews`, `/benchmark`. No marketing superlatives; no "beats", "best", "state of the art". No competitor names or logos on the landing page.
4. Motion: CSS only, subtle, `prefers-reduced-motion` respected. Mobile at 375 px must work with no horizontal scroll.

## Rules
- **You may touch only:** `viewer/app/page.tsx`, `viewer/components/landing/**`, `viewer/app/reviews/**`, `viewer/components/ReviewsDashboard.tsx`, `viewer/components/ReviewFinding.tsx`, `viewer/components/ReviewShell.tsx`, `viewer/app/benchmark/**`, new components under `viewer/components/pages/`, and the tests for these.
- **Don't touch:** `viewer/app/globals.css`, `viewer/app/layout.tsx`, `viewer/lib/**` (including `benchmark.ts` and `reviews.ts`), `viewer/app/graph/**`, any graph component, anything outside `viewer/`.
- **No new dependencies.** Next.js 16, React 19, Tailwind 4, lucide-react are available.
- Keep existing tests passing; update a test only where it asserts old markup, never where it asserts data, and say which in the report.
- Gates before each commit: `pnpm --filter @tracehound/viewer test`, `pnpm --filter @tracehound/viewer typecheck`, `pnpm --filter @tracehound/viewer build`, each as its own command.
- Small commits (`viewer(pages): …`), push to `ui/pages-v2`, open a **draft PR** titled `pages v2: landing, reviews, benchmark redesign`. Never merge.

## Report (final message)
What changed per page, any local tokens you added, tests you changed and why, the gates' results, the PR number and head sha.
