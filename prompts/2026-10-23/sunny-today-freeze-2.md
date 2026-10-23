# Sunny, by hand: 23 Oct (today), ReviewBench image + freeze-2

Written by chat, 23 Oct ~17:10 IST. Why today: HackerRank Orchestrate takes you from 24 Oct 18:00 to 25 Oct 18:00, and the v2 Cal.com run must start from the freeze tag before then.

Verified by chat before writing this:
- **#57 is merged** (16:25 IST): main = `0aeef06fa8742cc8a36581c3894e554b8748c965`, a merge commit (parents `8d0fb7a` + `4e3a0a8`). CI on main green.
- **ReviewBench #92 is merged** (approved by maintainer dapirian, 22 Oct). We can start test runs.
- No ReviewBench image has been built from `0aeef06` yet.
- The adapter's `pool-verify-v1` path calls the Action's `runPipeline`, so after #57 **all four pipelines** keep partial results. Amendment 5 covers every candidate.

Send chat the thing marked **→ send** after each step.

---

## 1. Amendment 5: reply yes or no
Chat writes it into P008 on "yes". **Don't start any portal test run before chat says it's written.**

## 2. Top up Nebius (~$50)
Console balance was $20.69. Do this before step 4: the smoke and every ReviewBench run bill the same account.

## 3. Rebuild the ReviewBench image on main
1. github.com/SunnyBagal/TraceHound → **Actions** → left list **ReviewBench image** → **Run workflow** → branch **main** → **Run workflow**.
2. Wait for green (~5–10 min). Open the run → **Summary**. Copy the line `Register this digest: ghcr.io/sunnybagal/tracehound-reviewbench@sha256:…`.

**→ send** the full digest.

## 4. Smoke on 3.5 with the new image
Image (built 17:09, run #8): `ghcr.io/sunnybagal/tracehound-reviewbench@sha256:9b9a5127317be79f4bc9f7e771b84b441d887f5cdd52e38e934b8657ceb84508`

Three runs (added 17:20: candidate 4 was never smoked on any image, candidate 3 never at all). Actions → **ReviewBench smoke** → **Run workflow** → branch **main**, image as above, three times:
   - A: pipeline `pool-raw`, model `nvidia/Nemotron-3.5-Super-VL`
   - B: pipeline `pool-verify-v1`, model `nvidia/Nemotron-3.5-Super-VL`
   - C: pipeline `pool-raw-x2`, model `nvidia/nemotron-3-super-120b-a12b`

Expect green: each row's status is `ok` or `ok partial=N/M`, and wall time well under 1800 s. About $0.40 for all three.

**→ send** a screenshot of the summary table. Red = stop and send it; don't add portal configs.

## 5. Portal: add configs on the new digest (after chat confirms amendment 5 is written)
In the ReviewBench vendor portal, reviewer `tracehound`. **Use "+ Add a configuration". Never "Register a reviewer".**

For each row: image = the new digest; delete the form's pre-filled label keys; add exactly two labels; save; then store credentials `NEBIUS_API_KEY` = the `reviewbench` key.

| candidate | `pipeline=` | `model=` |
|---|---|---|
| 1 | `pool-verify-v1` | `nvidia/nemotron-3-super-120b-a12b` |
| 2 | `pool-raw` | `nvidia/nemotron-3-super-120b-a12b` |
| 3 | `pool-raw-x2` | `nvidia/nemotron-3-super-120b-a12b` |
| 4 | `pool-verify-v1` | `nvidia/Nemotron-3.5-Super-VL` |
| 5 | `pool-raw` | `nvidia/Nemotron-3.5-Super-VL` |

Candidate 6 (`pool-raw-missed`) is **not** added yet: its model is whichever of 2 and 5 scores higher (P008 amendment 2), so it's added after those two test runs.

The two old configs on `ff22b972` (candidates 1 and 2, all-or-nothing): **never run them.** Delete or disable them if the portal lets you; if not, just leave them.

Don't start test runs yet. **→ send** a screenshot of the config list. Chat sends the run order.

## 6. Freeze-2: tag `review-freeze-2` on the #57 merge
`eval-freeze-2` already exists (4 Oct). Don't reuse that name.

In a plain terminal:
```bash
cd ~/Projects/TraceHound
git fetch origin --tags
git checkout main && git pull --ff-only
git rev-parse HEAD        # must print 0aeef06fa8742cc8a36581c3894e554b8748c965
git tag -a review-freeze-2 0aeef06fa8742cc8a36581c3894e554b8748c965 -m "review-freeze-2: #57 merge (068: 3.5 default, partial reviews)"
git push origin review-freeze-2
git ls-remote origin refs/tags/review-freeze-2   # → send this line
```
If `git rev-parse HEAD` prints anything else, stop and send it (someone pushed after #57). The tag still goes on `0aeef06…`, but chat checks first.

## 7. Re-pin dogfood (TraceHound) and demo (tracehound-demo)
Both workflows pin old shas and set no `model`, so they still run Nano generators. Pin both to the full freeze sha and set the model explicitly (lesson 20 Oct: every workflow that stands for the product names its model).

Do this in the GitHub web editor (pencil icon), committing **directly to main** for both:
- TraceHound: github.com/SunnyBagal/TraceHound/blob/main/.github/workflows/tracehound-review.yml
- demo: github.com/SunnyBagal/tracehound-demo/blob/main/.github/workflows/tracehound-review.yml

In each file, replace the `uses:` line and add the `model:` line, so the step reads exactly:
```yaml
      # Update this main-commit pin by hand after action changes.
      - uses: SunnyBagal/TraceHound/.github/actions/tracehound-review@0aeef06fa8742cc8a36581c3894e554b8748c965 # review-freeze-2
        with:
          nebius-api-key: ${{ secrets.NEBIUS_API_KEY }}
          model: 'nvidia/Nemotron-3.5-Super-VL'
          max-comments: '10'
          dry-run: 'false'
```
Commit messages: `ci: pin TraceHound review to review-freeze-2 (0aeef06), model 3.5` (same for both). If GitHub won't let you commit straight to main (branch protection), let it create a branch + PR and merge that PR with a merge commit.

**→ send** both commit shas.

**Don't touch demo PR #6. It must never be merged.**

## 8. Check the re-pin live (one review, ~$0.06)
On tracehound-demo, open **PR #2** (a seeded-bug PR) → **Close pull request** → **Reopen pull request**. The reopen runs the workflow from the new main pin.

Wait ~2–3 min for the new TraceHound review. **→ send** the PR #2 link. Chat checks that the review header lists 3.5 for both reviewers (verifier and fixes stay on 3 Super, by design) and reads the hidden footer.

## 9. Greptile consent email
Still open (R13: §1.4(a), no published results without consent). Say the word and chat drafts it in your Gmail as a draft for you to send.

---

## What chat does as each answer comes in
- "yes" → amendment 5 into P008, pushed, before step 5.
- After step 5: the test-run order (5 runs, then candidate 6).
- After steps 6–8: the v2 blind Cal.com run brief (relay), from the freeze tag, to launch before 24 Oct 18:00.
