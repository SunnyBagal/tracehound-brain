# Orchestrator playbook for Opus · 17 Nov 2026

Raw report, unreviewed, uncommitted; read-only at TraceHound `main` `c633b1c`, $0. No new rule: each line cites its
source, and the decision wins. "TS" is the test-set repo's `run/` folder.

## 0. Session start

1. Read `.agents/STATE.md`, `AGENTS.md`, `docs/runbooks/handover.md`, `docs/budget.md` "Rules", TS `briefs/_common.md`.
2. `grep '^## 076' docs/decisions.md | tail -1`: STATE's header must name that entry.
3. Read every TS `out/*.done` and `comms/*` newer than LOG's last line.

## 1. Lane brief

From `docs/plan/team-process.md` §2. Brief `TS/briefs/<lane>.<task>.md`; report `TS/out/<lane>.<task>.md`; `.done` last.

```markdown
# <lane>.<task>: <one line>
To: <one agent>  Sent: <UTC>  Report by: <UTC>  Cost: $0 | cap $<x>, line <name>  Side: dev | held-out
## Read first: _common.md; <file@sha or decision number>
## Task: numbered steps, one action and one result each
## You may write: exact paths
## Needs approval: command classes that can prompt, or "none"
## Done when: a count or an exit code
## Report: line 1 verdict or counts; full shas; what was not done
## If something does not match: stop and report the evidence
```

Before sending, each is yes:

1. `ls TS/briefs TS/out | grep <stem>` prints nothing.
2. Each sha equals `git rev-parse` of its ref.
3. Each "may write" path is the recipient's row in `AGENTS.md`.
4. Each file the task reads or passes as a flag passes `test -f` at that sha.
5. Each output the spec names has a producer: a script or a named lane.
6. A job over 90 minutes says "detached, own log, own `.done`".
7. A paid brief names its budget line, cap and every refusal source (total, run and per-PR caps).
8. A held-out task names a held-out-side recipient and stand-in (Process 6).

## 2. Review checklist (merge or paid run)

One "no" means no merge and no run.

1. The reviewer's first line is `GO <40-character sha>`; `git rev-parse <branch>` equals it.
2. `git diff --name-only main...<sha>` lists only the builder's paths, no `package.json`, and no file in a running
   scheduler's `pins.json`.
3. No frozen prompt, judge or protocol text changed without a decision entry; no provider outside `AGENTS.md`
   "Models and keys"; no test that can reach the network.
4. A guard's test shows the paid command did not run, not only that STOP printed (076r item 4).
5. The `GIT_*` check prints nothing. `pnpm test` and `pnpm typecheck` ran as separate commands, read before the
   commit. Known flake `test-set-run.test.ts` (061a): one rerun only.
6. Every rate follows from its row's counts; a gap on n under 100 is "within noise".
7. `git pull --rebase`, `git merge --no-ff <sha>`, gates, `git log --graph -3` shows the merge, push. No rebase
   after the merge; no force-push.
8. Paid run: a budget line exists; the job-start ledger value is in the job file; no other job's 2-hour
   console-check window is open.
9. Docs only: one grok pass. Brain: the pattern scan (case ids, held-out paths, PR numbers, fork names, ISO
   timestamps) finds 0; a failing note goes to codex (076g item 8).

## 3. Asking Sunny for a ruling

Sunny rules goals, money and test design (handover §3); artu1 decides operator detail and logs it within the hour
(076g item 8). Write `TS/comms/to-sunny.<n>.md` first; the file is the whole message.

```markdown
# to-sunny.<n>: <the question in one line>
Asked: <UTC>   Needed by: <UTC>   Blocks: <job or "nothing">
Facts: <counts only, each with file@sha or decision number>
Options: A) <action> → <cost, what changes>   B) …   (at most 3)
Recommend: <letter>, because <one sentence>
Default if no answer: <go on | hold>, applied at <UTC>
Touches: <files and owners>
```

- No case id, title, path or comment text.
- No bracketed placeholder; an answer with one is recorded "not given" and the hold stays (076q).
- A setup fact Sunny reports gets its time and, where one exists, a confirming probe (076h).
- The default never starts a window, posts a trigger, repeats a review or changes which comments count.
- Wait 2 hours inside 08:00–22:00 IST. The answer becomes a decision entry with "Touches:".

## 4. Stop conditions

Stop the job, write one line to Sunny, do nothing more on it:

1. Console ÷ ledger above 1.08 (1.26 once F1 freezes f = 1.17), read 2 hours or more after the last paid call
   (budget rule 5, 076r item 2).
2. A spend with no budget line, above its cap, or from unallocated for anything but 077.
3. A second paid process would start (076c item 1).
4. The claim file exists, or a brief's premise fails against the repository.
5. A step needs a held-out detail, a held-out pane, or a fork read between T0 and the lift.
6. A choice would change what a competitor tool sees or which comments count (handover §1 rule 3).
7. Scheduled T0 would pass the limit in 076k item 5.
8. `df -g` shows less free disk than the job's gate.

In the window nobody stops the scheduler by hand (handover §4):

- Pinned file changed (exit 3): restore the pinned bytes; never write new pins (H1 failure 15).
- Two held-out cases in a row fail on a provider error: canary, 3 tries, $0.40 (076b item 4).
- No `.done` by "Report by": read the pane once (never a held-out one), tell Sunny, reassign (Process 6).

Not a stop: a dev RETHINK. Tell Sunny that day; the tag goes ahead (076k item 4).

## 5. Mistakes already made → the rule

1. 10 Nov: a pane read and a borrowed script both reached held-out material → no `hc read` of a held-out lane; dry-run
   another lane's script first.
2. 13 Nov: bare `git status` in the test-set repo printed held-out names, twice → explicit paths only.
3. 9 Nov: a console reading a minute after a job looked like a leak; work frozen → §4.1.
4. 11 Nov: `pull --rebase` after a local merge flattened it → §2.7.
5. 11 Nov: one brief ran in two sessions → §1 check 1, the claim file.
6. 11, 17 Nov: codex sat on a permission prompt → "Needs approval" is never blank.
7. 14 Nov: a GO'd change touched a pinned file → §2.2; merge after the scheduler ends.
8. 14 Nov: a spec's readings had no producer; eight slots' readings lost → §1 check 5.
9. 14 Nov: "the apps cover the forks" was wrong; first triggers got no review → §3's probe.
10. 16 Nov: a ruling's placeholders were unfilled; a money guard printed STOP and exited 0 → §3, §2.4.
11. 16 Nov: the second judge's model was past its removal date → before a freeze, each frozen model id is in the
    provider's model list and off its deprecation notice.
12. 16 Nov: an edited bot comment was read as its original → use its first version and creation time (076s).
13. 16 Nov: STATE's header was four decisions behind → §0.2.
14. 17 Nov: a file two runbooks pass as a flag did not exist; the brain scan missed ISO timestamps → §1 check 4, §2.9.
15. 18 Oct: a 4-hour batch inside a session was killed; $1.44 lost → §1 check 6.
16. 24 Oct: a loose stop rule let an arm run; $3.33 lost → §1 check 7.
17. 8 Nov: a reliability test replayed the cache → fresh draws for the requests under test (073a).
18. 20–22 Oct: the product ran a different model from the benchmark → an adoption brief lists every difference.

## 6. Session handover

Rewrite STATE; one dated LOG line per decision; the `FEEDBACK.md` entry if Nebius was called; then
`TS/comms/orchestrator.handover.md` (none exists yet):

```markdown
# orchestrator.handover: <UTC>
Main: <full sha>   Last decision: <number>   Tests: <passed/failed> at <sha>
Money: console $<x> read <UTC>; ledger since $<y>; open caps: <job, cap, spent>
Running: <job, lane, pid, log path, next check UTC> | none
Open briefs: <stem, lane, Report by, claim, stand-in>
Waiting on Sunny: <to-sunny.<n>, asked UTC, default, applies UTC>
Held merges: <branch@sha, reason>
Stand-in: <who, until when>; limits as in Process 8
Not done: <task, why>
Brain: <notes; scan 0 yes/no; pushed yes/no>
```

1. STATE's header names the last decision heading; `wc -l .agents/STATE.md` prints 60 or less.
2. Every file in a new "Touches:" list is committed or in STATE's open tasks.
3. `git status --short` in TraceHound shows nothing unexplained.
4. No held-out detail, case id or secret is in STATE, LOG, the handover or the brain note.

## 7. Open at this handover

- STATE is stale: header 076r; `docs/decisions.md` ends at 076t (checked at main `e4fd83d`).
- `CLAUDE.md`, `AGENTS.md` and the brain's `inbox/README.md` require a codex GO for every brain change; 076g item 8
  says scan first. Fix the files or ask Sunny.
- E1's stop/go was read once: RETHINK (076t; grok's check has its `.done`). Confirm Sunny was told; company work
  pauses until the held-out result; the tag goes ahead.
