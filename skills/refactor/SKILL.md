---
name: refactor
description: Refactor code for clarity and simplicity without changing behavior, verified by existing tests
---

# Refactor

Improve the structure of existing code while preserving its behavior.

## Steps

1. Identify the target: the file/function the user names, or the changed files from `git diff main...HEAD --stat`
2. Read the code and the surrounding module to understand intent and conventions
3. Establish a safety net: locate existing tests covering the target and run them first to confirm a green baseline. If there are no tests, tell the user and recommend adding them (or run the `test-gen` skill) before refactoring risky code
4. Apply small, focused refactors
5. Re-run the tests after each meaningful change to confirm behavior is unchanged

## Refactors to Apply

- Extract repeated logic into a well-named helper (DRY)
- Replace deep nesting with early returns / guard clauses
- Rename unclear variables and functions
- Remove dead code and unused imports
- Simplify boolean and conditional expressions
- Break overly long functions into focused units

## Rules

- **Behavior must not change** — same inputs produce same outputs; public APIs stay stable unless the user asks otherwise
- Make surgical changes; do not rewrite working code wholesale or touch unrelated areas
- Match the surrounding style and patterns
- Keep each commit/change reviewable and self-contained
- If tests fail after a change, revert or fix it before continuing — report honestly if you cannot keep them green
