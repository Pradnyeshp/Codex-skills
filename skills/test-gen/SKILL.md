---
name: test-gen
description: Generate unit tests for changed or specified code, matching the project's existing test framework and conventions
---

# Test Generator

Generate tests for new or modified code that match the project's existing testing setup.

## Steps

1. Identify what to test:
   - If the user names a file or function, target that
   - Otherwise run `git diff main...HEAD --stat` (fall back to `git diff --stat`) and target the changed source files
2. Detect the test framework and conventions:
   - Look for config/deps: `package.json` (Jest, Vitest, Mocha), `pytest.ini`/`pyproject.toml` (pytest), `go.mod` (Go testing), `Gemfile` (RSpec), etc.
   - Read 1–2 existing test files to match naming, file location, imports, and assertion style
3. Write tests that follow the discovered conventions exactly
4. Run the test suite to confirm the new tests pass (use the project's test command)

## What to Cover

- The happy path for each public function/behavior
- Edge cases: empty inputs, boundary values, null/undefined, large inputs
- Error paths: invalid input, thrown exceptions, failed dependencies
- Any branch introduced by the diff

## Rules

- Match the existing framework, file naming (e.g. `*.test.ts`, `test_*.py`), and directory layout — do not introduce a new framework
- Use the project's existing assertion and mocking style
- One clear behavior per test; descriptive test names
- Do not test trivial getters/setters or third-party code
- After writing, run the tests and report pass/fail honestly — fix failures you introduced
