# TraceHound video: voice-over (2:50)

What Sunny says, nothing else. Shot numbers and times match `video-shots.md`. Read at a calm pace (about 150 words a minute); don't rush. The screen shows more than the voice says, and that's on purpose. Every number is from `numbers.md`. Never say a competitor's name.

v2 (30 Oct): the whole homepage and the whole benchmark page are scrolled; the seeded results are shown on the seeded-test card; cost moved into the benchmark shot.

---

## 1 · 0:00–0:10 · opening card · 23 words

AI agents write more pull requests than anyone can read. TraceHound reviews them, and every finding points to a line you can check.

## 2 · 0:10–0:32 · homepage · 51 words

TraceHound is an open source GitHub Action. It reviews pull requests with NVIDIA Nemotron three point five Super, served through Nebius Token Factory. It cuts a big change down to the few declarations that actually changed. And it maps your repository, where every connection points to a file and a line.

## 3 · 0:32–0:59 · pull request four · 59 words

Here's a real review of a planted bug, in a copy of my own app. One Nemotron reviewer reads only the diff. A second also reads the code around it. A third pass checks each finding before it's posted. This one caught a race. Two requests for the same new link can both find nothing, and both save it.

## 4 · 0:59–1:12 · pull request seven · 27 words

This catch used no model at all. The deterministic contract check found a renamed route that two other files still call. The pull request never touched them.

## 5 · 1:12–1:49 · benchmark · 84 words

I locked in the test first. Ten blind pull requests, scored once, with no tuning after. TraceHound's F score, which balances precision and recall, was fifty seven point one percent. That's statistically tied with four of five commercial reviewers, and ahead of one. Across all fifty, including forty I tuned on, it's behind the leader, tied with the next two, and ahead of the last two. It costs about six to seven cents a pull request. Every loss and limit is on this page.

## 6 · 1:49–2:13 · seeded-test card · 57 words

Then I opened thirty pull requests. Twenty two had a planted bug, eight were clean. It caught nine of the bugs, and stayed silent on all eight clean ones. Inside the diff, five of six. In files the change didn't touch, four of sixteen. And three of those four came from the contract check, not the model.

## 7 · 2:13–2:38 · graph · 55 words

This map is where TraceHound started. I built it to help a repair agent fix bugs. My own evaluation showed it didn't help, so I published that result. Now it does a simpler job. Pick a pull request, and the parts it touches light up. Click a connection, and you see the code behind it.

## 8 · 2:38–2:50 · install and end card · 20 words

To install it, add your Token Factory key as a secret, and save this one workflow file. MIT licensed. TraceHound.

---

| shot | time | seconds | words | spoken at 2.5 words/s |
|---|---|---|---|---|
| 1 | 0:00–0:10 | 10 | 23 | 9.2 s |
| 2 | 0:10–0:32 | 22 | 51 | 20.4 s |
| 3 | 0:32–0:59 | 27 | 59 | 23.6 s |
| 4 | 0:59–1:12 | 13 | 27 | 10.8 s |
| 5 | 1:12–1:49 | 37 | 84 | 33.6 s |
| 6 | 1:49–2:13 | 24 | 57 | 22.8 s |
| 7 | 2:13–2:38 | 25 | 55 | 22.0 s |
| 8 | 2:38–2:50 | 12 | 20 | 8.0 s |
| **total** | | **170** | **376** | **150 s** |

If a take runs long, cut in this order: "which balances precision and recall" (shot 5), "The pull request never touched them." (shot 4), "Now it does a simpler job." (shot 7). Never cut the last sentence of shot 6: the contract-check credit is required wherever a cross-file catch is mentioned.
