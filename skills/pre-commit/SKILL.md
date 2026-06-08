---
name: pre-commit
description: Set up git pre-commit hooks that lint, format, scan for secrets, and run fast checks before each commit, matching the project's tooling
---

# Pre-Commit Hooks

Add fast, reliable git pre-commit hooks that catch problems before they land, using the project's existing tools.

## Steps

1. **Detect the project's tooling** — read the manifest and config to find the real linters, formatters, and test runners (e.g. `eslint`/`prettier` from `package.json`, `ruff`/`black` from `pyproject.toml`, `gofmt`/`go vet` from `go.mod`, `rubocop` from `Gemfile`). Only hook tools the project actually uses
2. **Pick the mechanism** — prefer an established framework if present or idiomatic: the `pre-commit` framework (`.pre-commit-config.yaml`), Husky + lint-staged (JS/TS), or a plain `.git/hooks` script managed via `core.hooksPath`. Default to the ecosystem's common choice unless the user says otherwise
3. **Scope checks to staged files** — run formatters/linters only on changed files (e.g. lint-staged, `pre-commit`'s file filtering) so commits stay fast; reserve slow full-suite checks for CI
4. **Order the checks** — format → lint → type-check (if quick) → secret scan. Auto-fix and re-stage where safe (formatting), block on the rest
5. **Add a secret guard** — include a lightweight secret/credential check (e.g. `gitleaks`, `detect-secrets`) so credentials can't be committed
6. **Wire it up** — commit the hook config and add the install step (`pre-commit install`, `npm prepare`/Husky) so hooks activate on clone; document it in the README or setup docs
7. **Verify** — make a trivial staged change that violates a rule and confirm the hook blocks it, then confirm a clean change passes

## Rules

- Keep hooks **fast** — target well under ~10s; push slow/full test suites and builds to CI, not pre-commit
- Run checks against **staged files only**; don't reformat the whole repo on every commit
- Use the project's **existing** linters/formatters and their configured rules — don't introduce a new style or tool the team hasn't adopted
- Make hooks reproducible: pin tool/hook versions (e.g. `rev:` in `.pre-commit-config.yaml`) rather than floating `latest`
- Auto-fixers should re-stage their changes and let the commit proceed; non-fixable failures must exit non-zero and block the commit
- Ensure hooks install on clone (documented command or `prepare` script) — a hook nobody installs protects nobody
- Don't bypass-train the team: avoid noisy or flaky checks that push people toward `--no-verify`
- Never embed secrets in hook config; reference tools and configs, not credentials
