---
name: dependency-audit
description: Audit project dependencies for outdated, deprecated, and known-vulnerable packages and recommend safe updates
---

# Dependency Audit

Review the project's dependencies for security and freshness, and recommend safe updates.

## Steps

1. Detect the ecosystem from the manifest/lockfile:
   - `package.json` + lockfile (npm/pnpm/yarn), `requirements.txt`/`pyproject.toml`/`poetry.lock` (pip/poetry), `go.mod` (Go), `Cargo.toml` (Cargo), `Gemfile` (Bundler)
2. Run the native audit and outdated checks for that ecosystem, for example:
   - npm: `npm audit` and `npm outdated`
   - pnpm/yarn: the equivalent `audit` / `outdated` commands
   - Python: `pip list --outdated` (and `pip-audit` if available)
   - Go: `govulncheck ./...` (if available) and `go list -u -m all`
   - Cargo: `cargo audit` (if available) and `cargo outdated`
3. If a tool is not installed, note it and fall back to inspecting the manifest versions
4. Summarize findings and recommend a prioritized update plan

## Output Format

```text
## Dependency Audit

### Security (known vulnerabilities)
- `package@version` — <severity>: <advisory summary> → upgrade to `>=x.y.z`

### Outdated
- `package` — current `a.b.c`, latest `x.y.z` (<major|minor|patch> behind)

### Recommended actions
1. <ordered, safest-first update plan>
```

## Rules

- Prioritize **known vulnerabilities** (with severity) above routine outdated packages
- Flag **major** version bumps as potentially breaking — recommend reviewing changelogs/migration notes before applying
- Group patch/minor updates that are safe to batch
- Report the exact commands you ran and surface real tool output — do not fabricate advisories or versions
- Recommend updates; do not modify manifests or run upgrades unless the user asks
