---
name: monorepo
description: Manage a monorepo — workspaces, shared config, affected-only builds and tests, and clean dependency boundaries between packages
---

# Monorepo

Keep a multi-package repository fast and maintainable using the project's workspace tooling.

## Steps

1. **Detect the setup** — identify the tooling (npm/pnpm/Yarn workspaces, Nx, Turborepo, Bazel, Cargo workspaces, Go modules, Lerna) and read its config to learn how packages, tasks, and the dependency graph are defined
2. **Map the graph** — list packages and their internal dependencies; understand which packages depend on which so changes can be scoped
3. **Scope work to affected packages** — for builds/tests/lints, run only what a change actually impacts using the tool's affected/since mechanism (e.g. `nx affected`, `turbo run --filter`, `pnpm --filter`), not the whole repo
4. **Share config, not copies** — centralize lint/tsconfig/build presets and have packages extend them; avoid drift from per-package duplication
5. **Enforce boundaries** — keep dependency direction clean (no cycles, respect public entry points); use the tool's boundary/lint rules where available
6. **Leverage caching** — enable local and remote task caching keyed on inputs so unchanged packages aren't rebuilt
7. **Verify** — run the affected pipeline for a sample change and confirm only the right packages build/test, with the graph and caching behaving as expected

## Rules

- Run **affected-only** tasks for routine changes; reserve full-repo runs for release or config-wide changes
- **Share** base config (lint, tsconfig, build) and extend it per package — don't copy-paste config that will drift
- Keep the dependency graph **acyclic** and respect package public boundaries; don't reach into another package's internals
- Use the workspace tool's **caching** to skip unchanged work; key cache on real inputs
- Pin internal cross-package dependencies consistently with the workspace protocol the repo uses
- Don't hoist or duplicate dependencies in ways that break version expectations; follow the package manager's resolution model
- Make CI use the same affected/caching strategy as local so results match
- Keep changes scoped to the relevant package(s); avoid repo-wide churn unless that's the task
