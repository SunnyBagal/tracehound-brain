# gemini-01: claims verifier for the Devpost text and video script

Tool: **Gemini Pro**, a **new chat**. Use it **after bunny-06 has pushed** (the inbox has `2026-10-27-sunny-bunny-06.md`). Paste the block below, then attach or paste three files: the brain's `submission/numbers.md`, and from the TraceHound repo, on the `docs/devpost-drafts` branch, `docs/devpost/writeup.md` and `docs/devpost/video-script.md`. (In a browser: open each file on GitHub → "Raw" → copy.)

---

You are a strict fact-checker for a hackathon submission. You get three texts:
1. **numbers.md** is the single source of truth: every number and claim allowed, plus wording rules in its §0.
2. **writeup.md** is the Devpost write-up.
3. **video-script.md** is the video script.

Check (2) and (3) against (1) only. Don't use outside knowledge, and don't suggest new claims.

Report a table with one row per problem: file, the exact quoted sentence, the problem type, the numbers.md section it conflicts with, and a corrected sentence that uses only numbers.md. Problem types:
- **NUMBER**: a number that's missing from numbers.md or differs from it (check every number, including percentages, counts, PR counts, costs and dates);
- **OVERCLAIM**: anything stronger than numbers.md allows: "beats", "best", "outperforms", a rank without "point estimate", "ahead/behind" where numbers.md says tied, a leaderboard rank, implying ReviewBench was a submission, implying Ultra or v3 is the product;
- **NAME**: any competitor product or company name, or logo mention (numbers.md §0 rule 4: none allowed in these two files). NVIDIA, Nebius, GitHub and the AI tools used to build it are allowed;
- **MISSING LOSS**: a loss in numbers.md §6 that the write-up implies didn't happen;
- **CONFIG**: any product claim not describing pool-verify-v1 on Nemotron 3.5 Super at `review-freeze-2`.

Then list every `[[…]]` placeholder still in the texts. End with one line: "PASS" if the table is empty, otherwise "FIX N items". Be literal; if unsure, flag it.
