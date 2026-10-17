---
id: P005
title: Live demo on TraceHound itself plus a seeded-bug demo repo
date: 2026-10-16
kind: product
lane: chat
outcome: pending
call: too-early
judged_by:
judged_on:
repo_entry: none
pr:
---

# P005 · Demo: dogfood + seeded-bug repo

## Decided (Sunny, 2026-10-16)
- **TraceHound reviews its own PRs** with `.github/workflows/tracehound-review.yml`. The action is pinned to a main sha, so code from a PR never runs with the secret.
- **`SunnyBagal/tracehound-demo`:** a fresh-history copy of Recall @ `57d920e` (MIT). It gets 4 PRs that each add one of the seeded dev bugs from decision 044 (`eval/tasks/recall-dev-*/seed.patch`) under neutral titles, plus 1 control refactor.

## Why
- Judges may never test the demo (R3), so the video needs known-answer catches.
- Dogfooding shows real, ongoing use on a real repo.
- The seeds are public dev tasks with repro tests, so each catch can be checked.

## Risks
- The reviewer misses some seeds. Report that honestly; it is a demo, not a benchmark result.
- The seeds were tuning data for the repair agent (phase 2), not for the reviewer, so the reviewer was never tuned on them.
