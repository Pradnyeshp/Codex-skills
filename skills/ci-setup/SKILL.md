---
name: ci-setup
description: Generate or improve a CI pipeline (GitHub Actions, GitLab CI, etc.) that installs, lints, tests, and builds the project
---

# CI Setup

Create a continuous-integration pipeline that matches how the project is actually built and tested.

## Steps

1. **Detect the platform** — check for existing config (`.github/workflows/`, `.gitlab-ci.yml`, `.circleci/`, `azure-pipelines.yml`). If none, default to GitHub Actions unless the user says otherwise
2. **Learn the project's commands** — read the manifest and scripts to find the real install, lint, test, and build commands (e.g. `npm ci` + `npm test`, `pip install -r` + `pytest`, `go test ./...`, `cargo test`)
3. **Determine the matrix** — language version(s) and OS targets the project supports; keep it minimal but representative
4. **Write the pipeline** with clear stages: checkout → setup runtime (with dependency caching) → install → lint → test → build. Fail fast on each step
5. **Add sensible triggers** — on push to main and on pull requests, at minimum
6. **Cache dependencies** to keep runs fast (package manager cache keyed on the lockfile)
7. **Verify** the YAML is valid and the commands match ones that work locally

## Rules

- Use the **actual** commands the project uses locally — don't invent build steps that don't exist
- Pin action/image versions for reproducibility; avoid floating `latest`
- Enable dependency caching keyed on the lockfile to keep CI fast
- Keep the matrix lean — cover supported versions, not every permutation
- Never hardcode secrets; reference the platform's secret store and note which secrets the user must add
- Keep the pipeline readable and commented; one job's failure should clearly point to the cause
