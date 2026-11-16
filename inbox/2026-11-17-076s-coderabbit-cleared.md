# 076s: re-probe cancelled; code-review bot cleared for the tag (raw)

Ruled by the owner. The re-probe is cancelled and no review credit is spent: the first probe's timeline already met its pass condition. The bot's comment before the trigger was first the same "review skipped" notice every dev pull request got on opening; the "in progress" text was an edit made after the trigger. The earlier finding of a pre-trigger automatic review came from reading the edited comment and is corrected everywhere it was written.
Rule added to the held-out runbook: whether a pull request was reviewed before its trigger is decided from a comment's first version (the edit history) and its creation time against the trigger comment's time on the host's clock, never from the comment's current text.
The held-out-side check found no review-bot configuration file in any of the 58 trees that will be pushed (29 of 29 cases), so nothing can switch automatic review back on.
Reason: the pass condition was met; the error was in how a comment was read, not in the bot's behaviour.
