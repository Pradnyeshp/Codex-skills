---
name: code-review
description: Review the current branch diff for bugs, security issues, and quality problems with severity ratings
---

# Code Review

Review the changes on the current branch and produce actionable, prioritized feedback.

## Steps

1. Run `git diff main...HEAD` to see the full diff (fall back to `git diff --cached`, then `git diff` if the branch has no commits ahead of main)
2. If there is no diff, tell the user there is nothing to review
3. Review the changed lines only — do not flag pre-existing issues outside the diff
4. Group findings by severity and report them

## What to Check (in priority order)

1. **Security** — hardcoded secrets, SQL injection, XSS, command injection, auth bypasses, unsafe deserialization, missing input validation (OWASP Top 10)
2. **Correctness** — logic errors, off-by-one, null/undefined handling, race conditions, incorrect error handling, resource leaks
3. **Edge cases** — empty inputs, boundary values, concurrency, failure paths not handled
4. **Quality** — duplicated logic, dead code, unclear naming, missing tests for new behavior, violations of nearby conventions

## Severity Levels

- **Critical** — will cause a bug, data loss, or security hole; must fix before merge
- **Warning** — likely problem or risky pattern; should fix before merge
- **Suggestion** — optional improvement (readability, simplification)

## Output Format

```
## Code Review

### Critical
- `path/to/file.ext:42` — <issue and concrete fix>

### Warning
- `path/to/file.ext:88` — <issue and concrete fix>

### Suggestion
- `path/to/file.ext:120` — <improvement>
```

## Rules

- Every finding cites a file and line, states the problem, and proposes a concrete fix
- Omit empty severity sections
- If the diff is clean, say so plainly instead of inventing findings
- Be specific — no generic advice that could apply to any codebase
