# TraceHound video: voice-over (2:50)

What Sunny says, nothing else. Shot numbers and times match `video-shots.md`. Read at a calm pace (about 150 words a minute). Every number is from `numbers.md`. Never say a competitor's name.

Total: **364 words** in 170 seconds (about 146 seconds of speech, the rest is breathing room for clicks).

---

## 1 · 0:00–0:10 · opening card · 23 words

AI agents write more pull requests than anyone can read. TraceHound reviews them, and every finding points to a line you can check.

## 2 · 0:10–0:29 · homepage · 42 words

TraceHound is an open source GitHub Action. It reviews pull requests with NVIDIA Nemotron three point five Super, served through Nebius Token Factory. It also maps your repository. No model draws that map. Every connection points to a file and a line.

## 3 · 0:29–0:57 · pull request four · 65 words

Here's a real review of a planted bug, in a copy of my own app. One Nemotron reviewer reads only the diff. The second also reads the code around it. A third pass checks each finding before it's posted. This one caught a race. Two requests for the same new link can both find nothing, and both save it. It points at the exact line.

## 4 · 0:57–1:11 · pull request seven · 27 words

This catch used no model at all. The deterministic contract check found a renamed route that two other files still call. The pull request never touched them.

## 5 · 1:11–1:41 · benchmark · 60 words

I locked in the test before running it. Ten blind pull requests, scored once, with no tuning after. TraceHound's F score, which balances precision and recall, was fifty seven point one percent. That's statistically tied with four of five commercial reviewers, and ahead of one. Ten pull requests is a small test. The page says so, and lists every loss.

## 6 · 1:41–2:06 · seeded test · 60 words

Then I opened thirty pull requests. Twenty two had a planted bug. Eight were clean. It caught nine of the twenty two, and stayed silent on all eight clean ones. Inside the diff, it caught five of six. Bugs that break other files, only four of sixteen. And three of those four came from the contract check, not the model.

## 7 · 2:06–2:34 · graph · 55 words

This map is where TraceHound started. I built it to help a repair agent fix bugs. My own evaluation showed it didn't help, so I published that result. Now it does a simpler job. Pick a pull request, and the parts it touches light up. Click a connection, and you see the code behind it.

## 8 · 2:34–2:50 · install and end card · 32 words

To install it, add your Token Factory key as a secret, and save this one workflow file. It costs about six to seven cents a pull request, before tax. MIT licensed. TraceHound.

---

| shot | time | seconds | words | spoken at 2.5 words/s |
|---|---|---|---|---|
| 1 | 0:00–0:10 | 10 | 23 | 9.2 s |
| 2 | 0:10–0:29 | 19 | 42 | 16.8 s |
| 3 | 0:29–0:57 | 28 | 65 | 26.0 s |
| 4 | 0:57–1:11 | 14 | 27 | 10.8 s |
| 5 | 1:11–1:41 | 30 | 60 | 24.0 s |
| 6 | 1:41–2:06 | 25 | 60 | 24.0 s |
| 7 | 2:06–2:34 | 28 | 55 | 22.0 s |
| 8 | 2:34–2:50 | 16 | 32 | 12.8 s |
| **total** | | **170** | **364** | **146 s** |

If a take runs long, cut in this order: "It points at the exact line." (shot 3), "The pull request never touched them." (shot 4), "Ten pull requests is a small test." (shot 5). Never cut the last sentence of shot 6: the contract-check credit is required wherever a cross-file catch is mentioned.
