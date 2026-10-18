# grok-05: how top AI code reviewers get high precision (research)

Tool: **Grok Premium, a fresh chat.** Paste everything below the line. When it's done, paste Grok's whole answer back into the planning chat (Claude).

---

You are a research assistant. I build TraceHound, an open-source AI pull-request reviewer that runs on NVIDIA Nemotron 3 Super. On a 50-PR public benchmark (golden comments, LLM judge), our recall is close to the best tools, but our precision is 36–56% while Augment's and Cursor Bugbot's is 70–83%. I need concrete, sourced techniques to raise precision within 5 days, without fine-tuning.

Research and report:

1. **How each of these products reduces false positives,** from their own engineering blogs, docs, talks, or credible interviews (2025–2026): Augment Code review, Cursor Bugbot, Greptile, CodeRabbit, GitHub Copilot code review, Graphite Diamond, Macroscope, Qodo. For each, give:
   - the technique (for example multi-pass review, agentic repo exploration, self-critique, voting, severity gating, learned per-repo feedback, a confidence threshold, a comment cap per PR);
   - what they claim it achieves;
   - the link and the date.
2. **Papers (2024–2026) on LLM code review or bug-finding precision:** self-consistency or voting across samples, verifier or critic models, LLM-as-judge for review comments, retrieval or agentic context for review, false-positive filtering. For each, give the arXiv id or URL, the main result in one sentence, and whether it reports a measured precision gain.
3. **Cheap techniques we could test in under a day each,** ranked by expected precision gain per dollar. Mark each as either *measured* (cites a number) or *opinion*.

Rules:
- **Every claim needs a real, working link.** If you cannot find a source, say "no source found". Do not guess. One invented citation means this whole answer is discarded.
- Do not cite or open `codereview.withmartian.com` or `github.com/withmartian/code-review-benchmark`, or anything that shows per-PR results for Cal.com.
- Output: three tables (products, papers, quick wins), then 5 lines of "Opinion:".
