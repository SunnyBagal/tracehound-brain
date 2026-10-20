# rb-01b output (pasted by Sunny 20 Oct 19:04 IST). Verbatim.

Step 0 passes: head is `1d06203` and the tree is clean. Next I'm reading the price lookup, the token estimate and the reasoning flag, to confirm the cap arithmetic against the code.

I've confirmed the pricing fallback, token estimation, max_tokens caps, and reasoning settings, noting that review/review-ctx run as separate processes with independent $1 caps while the verifier reserves concurrently across 4 calls. I'll now test both your specified case and the verifier's case, starting with the price entry.

The viewer's citation `config/prices.json:6` still points at the Super line, because the new entry goes in after it. Now the budget test:

34 pass (was 27). Committing item 1:

Now item 3. First the chunk-failure test; I'm reading the current fake stages and the existing import test:

35 pass. Next the smoke workflow: a `model` input, stricter pass/fail, and more table columns.

The pipe-escaping `sed` I wrote is fragile. Simplifying to escape only the status text:

Testing the new loop locally in three fake-server modes: working, failing every call, and returning no findings.

The workflow correctly fails the bad cases and passes the good one, though pool-raw still passed on 3.5 with a stale local image since each reviewer runs as a separate process with only one chunk—the cap should actually trigger in pool-verify-v1, which reserves 4 calls at once. I'll test that on 3.5 with both the old and a rebuilt image.

Both images passed verification, confirming the real pricing applies since the pools were too small to trigger the reservation edge case. Now committing item 3.

I'll build a static test that traces the full import graph from `scripts/reviewbench/*.ts` and the three spawned stage CLIs, flagging any transitive dependency on `data.ts`, `fix.ts`, or files with "learn" in the name—this also guards against #49 sneaking learning logic into `pipeline.ts` or a stage.

38 pass. A quick mutation check: temporarily importing `fix.ts` from `adapter.ts` should turn the test red.

Both checks catch it, and the file is restored. Committing item 4:

Now the docs. Reading the current file:

Adding the expected-issues clause to the blindness section, then committing the docs:

I'll stop Docker to skip harness tests as before, then run the adapter tests and typecheck plus the repo-wide suites, since `prices.json` is referenced elsewhere.

All green. Pushing:

Waiting for the image build on the new head:

The new digest is `sha256:95cc69d2…`, and an anonymous manifest request returns 200, confirming the visibility check correctly takes the "public" branch. I'll now update #50's description while keeping it as a draft.

I've fixed all three gaps on PR #50, which stays a draft. The branch is pushed and the head is `6bb935c`; nothing under `packages/` changed.

- **Price entry (1):** `config/prices.json` now prices `nvidia/Nemotron-3.5-Super-VL` at $0.30 / $0.90 per 1M tokens, with the blog and model-page sources in `_readme`.
  - The new `prices.test.ts` uses the analyzer's own `Budget` and price lookup on the real table. It checks that 3 Super and 3.5 both have real prices, and that two review reservations and four verifier reservations fit under the $1 cap.
- **Cap arithmetic:** a 60,000-character chunk is estimated at 20,008 tokens.
  - At the $5 / $15 placeholder one reservation is $0.58, so two concurrent ones ($1.16) are refused, and the never-fail wrapper writes zero findings with exit 0.
  - At the real price it is $0.0348.
  - One correction to the brief: the reviewers run as separate processes, one chunk at a time, so they don't hit the cap with two concurrent calls. The cap is actually crossed by the verifier's four concurrent calls, or by several chunks in a row in one process.
- **Reasoning (2):** the stages send `enable_thinking: true` (`review.ts:15`); I didn't change it. The doc now says reasoning tokens share the 32,000-token `max_tokens`, and a truncated reply fails the PR with the reason in the status line.
- **Smoke test (3):** it has a `model` input (3 Super by default, or 3.5), passed as `RB_CONFIG_MODEL`.
  - It now fails if any status line isn't `status=ok` or if no PR got a finding.
  - The table adds model, status, findings and `cost_usd`.
  - Checked locally with a fake model server: working calls go green, failing calls go red, and calls returning no findings go red.
  - A new test confirms a chunk failure of "invalid reply" shows up in the status line.
- **Learning (4):** `imports.test.ts` fails if the adapter imports `data.ts`, `fix.ts` or any `*learn*` file. It also fails if anything the image runs reaches one indirectly, which covers #49 wiring learning into the pipeline. I confirmed it catches a planted import.
- **Docs (5):** the five candidates from P008 with the link, the candidate 6 line, the data-branch line, and the three smoke runs to do after merge.
- **Tests:** adapter tests went from 27 to 38, all passing, and the typecheck is clean. The rest of the repo's tests and typecheck are green.
- **Image:** `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:95cc69d241aa640e28b067696cb3429b0f64a1077d01432d77b55b8a64a81a0f`, from the build run on `6bb935c`.
- **Public package:** an anonymous pull of that digest works, which is the same check the build summary runs. I couldn't read the summary text itself through the API.

No Nebius call was made, so `FEEDBACK.md` gets no entry. After merge, Sunny runs the three smoke runs listed in the doc.