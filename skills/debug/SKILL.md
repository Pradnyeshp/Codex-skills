---
name: debug
description: Systematically diagnose and fix an error, stack trace, or failing test by finding the root cause first
---

# Debug

Diagnose a bug methodically and fix the root cause — not the symptom.

## Steps

1. **Gather the evidence** — get the exact error message, stack trace, failing test name, or a precise description of the wrong behavior. Ask for it if not provided
2. **Reproduce** — find or write the smallest command/test that reliably triggers the failure. Run it and confirm you see the reported behavior. If you cannot reproduce, say so and ask for steps
3. **Locate** — read the top frame of the stack trace, open that file, and trace backward to where the bad value or state originates
4. **Form a hypothesis** — state the single most likely root cause in one sentence before changing anything
5. **Confirm** — verify the hypothesis (add a focused log/print, inspect the value, or write a failing assertion) so you are fixing the real cause
6. **Fix** — apply the smallest change that addresses the root cause
7. **Verify** — re-run the reproduction and the surrounding test suite to confirm the fix and no regressions

## Rules

- Fix the **root cause**, not the symptom — do not paper over with try/catch or special-casing unless that is genuinely correct
- Change one thing at a time; re-test between changes
- Do not guess-and-check randomly — every change follows from a stated hypothesis
- Remove any temporary debug logging before finishing
- If the root cause is outside the user's control (a dependency bug, environment issue), say so clearly and suggest a workaround
- Report honestly if the fix is a mitigation rather than a true resolution
