---
name: env-setup
description: Produce a reproducible developer setup — required tooling, env vars, install/run steps — and a getting-started guide for new contributors
---

# Env Setup

Make it easy for a new contributor to go from clone to running the project.

## Steps

1. **Inventory the requirements** — read the manifests and config to determine: runtime/language versions, package manager, services (database, cache, queue), and required environment variables
2. **Find the real run path** — identify the actual install, build, run, and test commands from scripts/Makefile/docs, and verify they're current (not stale README instructions)
3. **Capture version pins** — note exact tool versions needed and check for a version file (`.nvmrc`, `.python-version`, `.tool-versions`, `go.mod`, `rust-toolchain`). Suggest adding one if missing
4. **Document environment variables** — list every required/optional var with a short description and a safe example. Create or update a `.env.example` (never with real secrets)
5. **Cover external services** — provide the simplest way to run them locally (e.g. a `docker-compose` snippet or setup commands)
6. **Write the getting-started steps** — an ordered, copy-pasteable sequence: prerequisites → clone → install → configure env → start services → run → test
7. **Verify** the steps actually work by following them on the commands available

## Output Format

```text
## Getting Started

### Prerequisites
- <runtime> <version>, <package manager>, <services>

### Setup
1. <clone>
2. <install deps>
3. cp .env.example .env  # then fill in values
4. <start services>
5. <run>
6. <run tests>

### Environment variables
- `VAR_NAME` (required) — what it's for; example: `...`
```

## Rules

- Document **exact** versions and pin them with a version file where the toolchain supports it
- Provide a `.env.example` with safe placeholders — never commit real secrets or credentials
- Steps must be copy-pasteable and ordered so a fresh clone runs end to end
- Use the project's real commands; verify they work rather than copying stale docs
- Prefer one documented setup path over listing many alternatives; mention OS-specific differences only where they matter
- Keep it current — flag any setup instructions you find that are already out of date
