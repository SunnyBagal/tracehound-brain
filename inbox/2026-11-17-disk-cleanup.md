# Disk cleanup before the dev table run (raw)

Ruled by the owner. Free space was about 1.4 GiB after a backup bundle filled the disk; the gate for the next paid runs is at least 10 GiB free.
Deleted, each with the tool's own command where one exists:
- pip cache: `pip3 cache purge`, 3,295 files, 2,180.7 MB.
- Yarn classic cache: `yarn@1 cache clean`, 1,451,712 KB (about 1.39 GiB).
- Playwright browsers: `playwright uninstall --all`, about 1.3 GB (1,706,760 KB before, 403,080 KB left that it reports as still in use); no project step of the dev table, the held-out run, its scoring or the test suite uses Playwright.
- Browser and IDE caches, directories under the user Caches folder only (nothing under Application Support, no profiles or settings): JetBrains 1,916,600 KB, Brave 1,904,532 KB, Dia 1,359,568 KB.
Free afterwards: about 11.1 GiB. Not touched, sizes reported for the owner: npm cache 20 GB, pnpm store 2.7 GB, Homebrew cache 438 MB, the Docker disk image 50 GB, Xcode build data none, Trash not readable.
Reason: a full disk during a run would corrupt its records.
