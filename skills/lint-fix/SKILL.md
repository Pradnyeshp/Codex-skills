---
name: lint-fix
description: Run the project's linters and formatters, then fix the reported issues without changing behavior
---

# Lint Fix

Bring the code into compliance with the project's linting and formatting rules.

## Steps

1. **Detect the tooling** — look for config files and scripts:
   - JS/TS: ESLint (`.eslintrc*`), Prettier (`.prettierrc*`), Biome (`biome.json`); check `package.json` scripts
   - Python: Ruff (`ruff.toml`/`pyproject.toml`), Black, Flake8, isort
   - Go: `gofmt`/`goimports`, `golangci-lint`
   - Rust: `rustfmt`, `cargo clippy`
2. **Run the checks** — execute the lint and format commands the project already defines (prefer the `lint`/`format` npm/make scripts over ad-hoc invocations)
3. **Apply safe auto-fixes first** — `eslint --fix`, `ruff check --fix`, `gofmt -w`, `cargo clippy --fix`, formatters
4. **Resolve the rest by hand** — fix remaining warnings/errors that auto-fix can't, one rule at a time
5. **Re-run** until the lint/format checks pass clean
6. **Verify behavior** — run the test suite to confirm nothing changed functionally

## Rules

- Use the project's configured tools and rules — do not introduce new linters or rewrite configs unless asked
- Fixes must be **behavior-preserving**; formatting and style only
- Do not blanket-disable rules. If a warning is a genuine false positive, suppress it narrowly (single line/symbol) with a brief reason
- Keep formatting changes separate from logic changes when possible, so diffs stay reviewable
- Report what you ran, what was auto-fixed, and what you fixed manually
- If a lint rule conflicts with a formatter, follow the project's documented precedence
