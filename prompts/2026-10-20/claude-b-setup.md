# Claude B: setup of the second Pro account (20 Oct)

Claude B is a **builder lane**: a second Claude Code login on the same Mac, in its own clone. It needs no memory or project history: every job prompt is self-contained, and the repo's `CLAUDE.md` loads by itself. The planning chat stays on account A.

## 1. Terminal (one time, about 5 minutes)
```bash
# the brain, so Bunny can drop its reports in inbox/ (skip if you already have it)
git clone https://github.com/SunnyBagal/tracehound-brain ~/Projects/tracehound-brain

# a separate clone, so B never touches relay's checkout
git clone https://github.com/SunnyBagal/TraceHound ~/Projects/TraceHound-claude2
cd ~/Projects/TraceHound-claude2 && corepack enable && pnpm install --frozen-lockfile

# account B gets its own config dir; account A stays in ~/.claude
mkdir -p ~/.claude-b
echo "alias claudeb='CLAUDE_CONFIG_DIR=\$HOME/.claude-b claude'" >> ~/.zshrc
source ~/.zshrc

# log in as the NEW account (a browser opens; pick the new account)
cd ~/Projects/TraceHound-claude2 && claudeb
#   inside it, run /login if it doesn't ask. Then /status should show the new account's email.
```

Check account A afterwards: in another terminal, `claude auth status --text` must still show your original account. Each config dir gets its own macOS Keychain item, so the two logins shouldn't clash. If A got logged out, tell the chat.

- **Never** `export CLAUDE_CONFIG_DIR` in `.zshrc`. Use the alias only.
- **No `.env` in this clone.** The Nebius spend ledger lives per checkout (`.tracehound/`), so B's spend would be invisible to the $45 total cap. B's jobs are $0 unless a brief explicitly says otherwise.
- GitHub: B pushes with the same `gh` / git credentials as your other checkouts. Nothing extra is needed.

## 2. Paste this first, in every new `claudeb` session
> You are a builder on TraceHound (an open-source AI PR reviewer). Rules:
> - Work only in `~/Projects/TraceHound-claude2`. Read `CLAUDE.md` and `AGENTS.md` first.
> - Never open `SunnyBagal/linea-seeded`, `SunnyBagal/recall-seeded` or `SunnyBagal/tracehound-seeds`, or ReviewBench's `golden/`.
> - Make no Nebius or model API calls unless the brief gives a cap. There is no `.env` here, on purpose.
> - Never edit `~/Projects/tracehound-ops/`.
> - Open PRs as drafts. Never merge, never force-push.
> - When done, write your final report verbatim to `~/Projects/tracehound-brain/inbox/<YYYY-MM-DD>-bunny-<job>.md`, following `inbox/README.md`, and push it. Edit nothing else in the brain.
> - Follow the brief exactly. If a step's check fails, STOP and report. End with: the PR number, head sha, one line per commit, and test counts.

## 3. First job: 066 (docs only, $0). Moved here from relay, which is busy with rb-02
Paste this, then everything in `prompts/2026-10-19/066-results-docs.md` from the line "Decision number: 066" down:
> This brief was written for the relay tool. You are not running under relay: ignore the `caffeinate … relay.ts` command and anything about `lanes.json`. Your folder is `~/Projects/TraceHound-claude2`, not `~/Projects/TraceHound`. Everything else applies as written.

Model: **Sonnet** is fine for 066: it's mechanical docs with exact wording given. Use `/model` to switch.

## Next jobs for B (the chat writes them)
- **int-01:** wire contracts (#47) and learning (#49) into the Action, after #49 merges. Use the strongest model.
- UI or README jobs after Codex ends on 22 Oct.
