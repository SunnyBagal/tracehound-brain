# TraceHound handoff, 20 Oct 2026, 21:00 IST

Paste this file into a new Claude chat as the first message, with: "Continue TraceHound from this handoff."

## Your role (planning chat)
- Sunny Bagal is building **TraceHound**, an open-source AI PR reviewer (GitHub Action) on NVIDIA Nemotron via Nebius Token Factory, for the **Nebius × NVIDIA hackathon**. Deadline 30 Oct 2026, 22:30 IST; the target is to submit on 29 Oct.
- **You plan; you don't build.**
  - Every prompt is a file in the brain (`prompts/<date>/<job>.md`), pushed before Sunny pastes it.
  - Always say which tool, folder and session it goes in. Send the file with SendUserFile too.
- **Check every PR from GitHub before Sunny merges it.**
  - Fetch `refs/pull/N/head` into a clone of the public TraceHound repo (read-only: `add_repo` access read).
  - Read the diff, recompute tables, and probe deterministic logic with adversarial inputs.
  - Run the relevant tests on Node 22 with vitest. Put **HOLD #N** first when something must not merge.
  - Keep checks lean: no full test suites unless the PR touches shared code.
- **Brain:** `SunnyBagal/tracehound-brain`. Attach it with `add_repo` (access push).
  - File every agent output verbatim as `prompts/<date>/<job>.out.md`.
  - **Process `inbox/` first**: builders drop their reports there. `git mv` each report to its `.out.md` file.
  - Update NOW, prs, timeline and lessons after each event, then push.
  - The brain is public: no secrets, no Cal.com comment text.
- **Style:** short, blunt, honest. Answer what Sunny asks. Don't build unrequested deliverables; offer extras in one line.

## Read in order
1. `NOW.md` (the top sections are current; older sections below them are history).
2. `decisions/P008-reviewbench-preregistration.md`: amendments 1–4.
3. `research/R11`–`R14`: R11 grok/ChatGPT levers, R12 the ReviewBench portal, R13 tool terms, R14 the Devpost checklist.
4. `lessons.md`, top 3 entries.
5. `inbox/README.md`: the builder reporting rules.

## Lanes (names are Sunny's)

| Lane | What it is | Folder | State |
|---|---|---|---|
| **Sunny** (by hand) | merges, portal, smoke runs, console | — | see "Pending with Sunny" |
| **Sunny's Claude Code (relay)** | account A, the relay tool | `~/Projects/tracehound-ops` → `~/Projects/TraceHound` | **067 blocked** (3.5 returns 404). Sunny was told to Ctrl-C the relay (it polls CI on a branch with no PR) |
| **Bunny** | **second Claude Pro account**, `claudeb` alias (`CLAUDE_CONFIG_DIR=~/.claude-b`), no `.env` (no Nebius calls) | `~/Projects/TraceHound-claude2` | 066 done (#52); **bunny-02 judges' README** next (new session, strongest model) |
| **Codex** | ChatGPT Codex, **ends 22 Oct** | `~/Projects/TraceHound-codex` | **codex-10 int-01** (contracts in the Action) given to Sunny; not confirmed started |
| **Claude cloud** | claude.ai/code, **~$52 credit left before rb-02 ran there** (expires 5 Nov). A big builder job costs about $20–30 | — | Ultrareview #2 on int-01's PR once it opens; otherwise reserve |
| **Grok bots** | Planner, Researcher, Verifier, Isabella, Ninja, Executioner; web only, no repo access. Researcher → Verifier pairs; you link-check | — | jobs A, D and E done (R12–R14). Verifier B (post drafts) **never pasted**. Planner gives a daily 9:00 digest |

Builders (Bunny, relay) write their final report to the brain's `inbox/` and push only that file. **They never edit NOW, prs, results, decisions or research.**

## State (verified 20 Oct 21:00)
- **TraceHound main `95f79f0`.** Merged today:
  - #50: ReviewBench image + rb-01b (3.5 price, smoke test that can fail, learning kept out of the image);
  - #49: learning, after codex-09b fixed the matcher (identifiers kept);
  - #51: rb-02, `review-missed-v1` + `pool-raw-missed`, prompt sha256 `a5a2115f…53bc`, recorded in P008.
- **Open:**
  - **#52** (Bunny, 066 results docs, head `e91a268`): **cleared to merge**.
  - **#42** (old spike): Sunny to close it unmerged.
- **ReviewBench** (P008, six candidates):
  1. pool-verify-v1, 3 Super;
  2. pool-raw, 3 Super;
  3. pool-raw-x2, 3 Super;
  4. pool-verify-v1, 3.5;
  5. pool-raw, 3.5;
  6. pool-raw-missed. Its model is the winner of 2 vs 5; it needs a ≥3.0 margin; its cutoff was 23 Oct 12:00, and it **is merged**.
  - Amendment 3: if the test-run judge isn't funded, only 2, 5 and 6 run.
  - Amendment 4: if 3.5 can't be called when the test runs start, 4 and 5 are dropped and 6 runs on 3 Super.
  - **Never open `golden/`.** Test-run "misses" are read only to confirm runs worked.
- **Image:** `ghcr.io/sunnybagal/tracehound-reviewbench`, public. #50's digest is `sha256:95cc69d2…`. **It must be rebuilt on main** after #51 (the "ReviewBench image" workflow, dispatched on main).
- **Test-run judge (R12):** test runs are judged at **our** cost (Claude Sonnet 5, ~$3 each). The final run is paid by ReviewBench. Sunny "might fund" an Anthropic API key (~$20). A new digest needs no re-approval; approval takes ≤ 1 business day; the final run is permanent.
- **Nemotron 3.5 Super VL:** listed in `GET /v1/models`, but **every chat call returns 404 "does not exist"** (067, 35/35, $0). The console shows "Nemotron 3.5 Super VL", public, BF16, 1,024K context. Sunny was asked to:
  - try it in the playground;
  - copy the exact API id and base URL from its code sample;
  - contact Nebius support.

  067 is on branch `build/067-dev-3.5` (`cf5b70b`): the plan is frozen, the baseline recomputed (v1 F 42.28 / P 40.52, x1 51.29), and run 7 is void. Resume with `--restart` or a new id. A new id also needs a `config/prices.json` key, which is outside relay's lane.
- **Nebius ledger:** $25.82 of the $45 cap (067 spent $0). Top up about $25 before the ReviewBench final, then cap ~$75.
- **v2 for freeze-2 (chat decision):** the Action = pool-verify-v1 + **contracts** (int-01, default on, fail-soft) + `learn` (default off). The gate and the evidence verifier stay **out** (065 lost).
- **Public claim wording (066, #52):** "On 10 blind PRs, statistically tied with Greptile and Cursor; run-1 F 58.3 vs CodeRabbit 36.7, but not a significant lead (the 95% interval includes 0; ahead in 96% of resamples). 4th of 6 across all 50 under both judges. Every loss is published." **Never "3rd of 6" as a ranking, never "beats".**
- **Devpost (R14):**
  - the video has **no competitor names or logos** (trademark rule);
  - the demo must work without a judge's key (dashboard + live PR reviews);
  - MIT LICENSE exists;
  - feedback is required (FEEDBACK.md);
  - "what changed since 26 Aug" is needed.
- **Tool terms (R13):**
  - Cursor allows publishing benchmarks with a full replication kit;
  - Greptile §1.4(a) bars sharing "any results" without OK, so Sunny should email them;
  - CodeRabbit: Sunny keeps it. Don't re-argue.

## Pending with Sunny
1. Merge #52; close #42.
2. Rebuild the ReviewBench image on main, then the smoke runs:
   - pool-raw on 3 Super;
   - pool-verify-v1 on 3 Super;
   - pool-raw-missed on 3 Super;
   - pool-raw on 3.5, **only once 3.5 answers**.

   Send the summaries.
3. Register in the ReviewBench portal; decide on the Anthropic API key.
4. Fix 3.5: playground, exact id, Nebius support.
5. Start Codex on `prompts/2026-10-20/codex-10-int-01-contracts.md` (a new Codex task; must land by 22 Oct). Then run Ultrareview #2 on its PR (cloud).
6. Start Bunny on `prompts/2026-10-20/bunny-02-judges-readme.md` (a new `claudeb` session).
7. Email Greptile for consent to publish its seeded-test result.
8. Paste Verifier B (the X/LinkedIn drafts) if wanted. **Posting is planned for about 26 Oct**, once there are fresh numbers.

## Next jobs to write
1. Check int-01's PR, plus the Ultrareview #2 findings.
2. 067 resume prompt, once 3.5 works (with the new id and a prices entry).
3. Freeze-2 (24 Oct): a tag on main after int-01. Then the **one blind Cal.com run of v2** (relay) and the **seeded test** (25 Oct: install the Action at the freeze-2 pin plus CodeRabbit, Greptile and Bugbot on both mirrors; Sunny runs `open-prs.sh`; scoring uses the private key `a4a6c83`, with hashes in NOW).
4. ReviewBench test runs → selection by the P008 rule → the final run.
5. Fill bunny-02's README placeholders; the results doc; the Grok final audit (26 Oct); the Devpost write-up; the video (no competitor names); submit 29 Oct.

## Side project
TariffLens starts 30 Oct.
