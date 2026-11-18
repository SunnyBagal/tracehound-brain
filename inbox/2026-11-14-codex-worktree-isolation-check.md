# worktree isolation check

Observed 2026-11-14T16:38:58.783747+00:00. Repository checks were read-only; no edit, commit, checkout, switch or push. This requested raw inbox note is the sole write.

- Artu 1: /Users/sunny / N/A (Herdr t9 cwd; not a Git repository; project checkout unconfirmed)
- Bunny: /Users/sunny/Projects/TraceHound / main (Herdr t15)
- Codex: /Users/sunny/Projects/TraceHound / main (Herdr t14; pwd and Git top level agree; clean,0 files)
- Shared folders: yes (/Users/sunny/Projects/TraceHound, Bunny and Codex)
- Shared branches: yes (main in that same checkout, Bunny and Codex)
- Worktree or separate clone for each: Artu1 has no Git cwd confirmed; Bunny and Codex share TraceHound's original checkout, not isolated linked worktrees. TraceHound registers32 worktrees, including separate task worktrees, but their existence does not establish current pane use.

TraceHound-claude2 is not in TraceHound's worktree list. Its branch is docs/devpost-drafts. Its physical Git common directory is /Users/sunny/Projects/TraceHound-claude2/.git, distinct from /Users/sunny/Projects/TraceHound/.git: a separate clone. No pane-to-clone assignment was established.

Herdr metadata for TraceHound workspace: t9 claude-code /Users/sunny; t14 codex /Users/sunny/Projects/TraceHound; t15 claude-code /Users/sunny/Projects/TraceHound; t1g grok /Users/sunny/Projects/tracehound-testset. Finished pane t1q is zsh in /Users/sunny/Projects/TraceHound. A separate Test workspace has t1p zsh in that folder. These are reported pane working directories, not proof of every subprocess's -C/workdir target. No pane content was read and no agent was prompted.
