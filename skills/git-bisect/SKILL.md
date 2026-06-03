---
name: git-bisect
description: Find the exact commit that introduced a regression using git bisect with a reliable reproduction test
---

# Git Bisect

Pinpoint the commit that introduced a bug by binary-searching git history.

## Steps

1. **Define good vs bad** — confirm the current (bad) state shows the bug, and identify a known-good commit/tag/date where it didn't. Ask if the good revision is unknown
2. **Build a reliable check** — get or write the smallest command or test that returns clean pass/fail for the bug. Bisect is only as good as this signal
3. **Start the bisect** — `git bisect start`, mark `git bisect bad` (current) and `git bisect good <ref>`
4. **Test each step** — at each commit git checks out, run the check and mark `git bisect good`/`git bisect bad`. If a commit can't be tested (won't build), `git bisect skip`
5. **Automate when possible** — if the check is a single command with a proper exit code, use `git bisect run <command>` to let git drive the whole search
6. **Identify the culprit** — git reports the first bad commit. Read its diff to understand what changed
7. **Clean up** — `git bisect reset` to return to the original branch, then summarize the offending commit and the likely fix

## Rules

- The pass/fail check must be **deterministic** — a flaky test makes bisect point at the wrong commit
- Prefer `git bisect run` with a command that exits 0 (good) / non-zero (bad); use exit code 125 for "skip/untestable"
- `git bisect skip` commits that can't be built or tested rather than guessing
- Always `git bisect reset` when done so the working tree is restored
- Report the culprit commit hash, its author/message, and the specific change that caused the regression — not just the hash
- Don't modify history or commit anything during the bisect; this is diagnosis, not the fix
