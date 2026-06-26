# Codex-Skills

Collection of helpful Codex CLI skills for software development.

## Skills

| Skill | Description |
| ----- | ----------- |
| [Commit Message](/skills/commit-message/SKILL.md) | Generate conventional commit messages from staged changes |
| [PR Description](/skills/pr-description/SKILL.md) | Generate PR title and description from branch diff |
| [Changelog](/skills/changelog/SKILL.md) | Generate changelog entries from recent commits |
| [Code Review](/skills/code-review/SKILL.md) | Review the branch diff for bugs, security issues, and quality problems with severity ratings |
| [Test Gen](/skills/test-gen/SKILL.md) | Generate unit tests for changed code, matching the project's framework and conventions |
| [Refactor](/skills/refactor/SKILL.md) | Refactor code for clarity without changing behavior, verified by existing tests |
| [Dockerize](/skills/dockerize/SKILL.md) | Generate a production-ready Dockerfile and .dockerignore for the project |
| [Debug](/skills/debug/SKILL.md) | Systematically diagnose and fix an error, stack trace, or failing test by root cause |
| [Explain](/skills/explain/SKILL.md) | Explain unfamiliar code or a codebase area in plain language for onboarding |
| [Doc Gen](/skills/doc-gen/SKILL.md) | Add docstrings and inline documentation matching the language's conventions |
| [Dependency Audit](/skills/dependency-audit/SKILL.md) | Audit dependencies for outdated and known-vulnerable packages and recommend updates |
| [Optimize](/skills/optimize/SKILL.md) | Profile a slow path, find the real bottleneck with measurements, and apply a verified performance fix |
| [API Design](/skills/api-design/SKILL.md) | Design REST/GraphQL endpoints — paths, schemas, status codes, and errors — following project conventions |
| [Lint Fix](/skills/lint-fix/SKILL.md) | Run the project's linters and formatters and fix the reported issues without changing behavior |
| [Type Annotate](/skills/type-annotate/SKILL.md) | Add or improve static type annotations and verify with the type checker |
| [Scaffold](/skills/scaffold/SKILL.md) | Scaffold a new component, module, or feature with boilerplate, tests, and wiring matching the codebase |
| [Release](/skills/release/SKILL.md) | Cut a versioned release — determine the bump, update version files and changelog, and tag it |
| [Migrate](/skills/migrate/SKILL.md) | Write a safe, reversible database schema or data migration following the project's migration tool |
| [CI Setup](/skills/ci-setup/SKILL.md) | Generate or improve a CI pipeline that installs, lints, tests, and builds the project |
| [Accessibility](/skills/accessibility/SKILL.md) | Audit UI code for a11y issues against WCAG and suggest concrete fixes |
| [Logging](/skills/logging/SKILL.md) | Add structured, leveled logging and basic observability using the project's logging library |
| [Git Bisect](/skills/git-bisect/SKILL.md) | Find the exact commit that introduced a regression using git bisect with a reliable test |
| [Env Setup](/skills/env-setup/SKILL.md) | Produce a reproducible developer setup and getting-started guide for new contributors |
| [Readme](/skills/readme/SKILL.md) | Generate or update a clear, accurate project README that helps a newcomer install and use the project |
| [Secrets Scan](/skills/secrets-scan/SKILL.md) | Scan the codebase and git history for hardcoded secrets and credentials, then remediate and prevent leaks |
| [Flaky Test](/skills/flaky-test/SKILL.md) | Diagnose and stabilize a flaky or intermittently failing test by finding the real source of nondeterminism |
| [Dead Code](/skills/dead-code/SKILL.md) | Find and safely remove unused code — dead functions, unreachable branches, unused imports and dependencies |
| [Error Handling](/skills/error-handling/SKILL.md) | Harden error handling and edge cases — validate inputs, handle failures explicitly, and fail safely |
| [Pre-Commit](/skills/pre-commit/SKILL.md) | Set up fast git pre-commit hooks that lint, format, and scan for secrets, matching the project's tooling |
| [Benchmark](/skills/benchmark/SKILL.md) | Write and run reproducible performance benchmarks with stable, statistically meaningful measurements |
| [Feature Flag](/skills/feature-flag/SKILL.md) | Gate functionality behind feature flags for safe rollout, with a plan to remove the flag later |
| [i18n](/skills/i18n/SKILL.md) | Extract hardcoded strings and wire up internationalization and localization using the project's i18n library |
| [Monorepo](/skills/monorepo/SKILL.md) | Manage a monorepo — workspaces, shared config, affected-only builds and tests, and clean dependency boundaries |
| [Caching](/skills/caching/SKILL.md) | Add or improve caching safely — pick the right layer and key, set TTL and invalidation, and verify hit rate |
| [Observability](/skills/observability/SKILL.md) | Add metrics, tracing, and actionable alerts so a service's health and behavior are visible in production |
| [Rate Limiting](/skills/rate-limiting/SKILL.md) | Protect a service with rate limiting and throttling — pick the algorithm and scope, set limits, return correct headers, and fail safely |
| [Config Management](/skills/config-management/SKILL.md) | Structure application configuration safely — layer sources, validate at startup, keep secrets out of code, and document every setting |
| [Deployment](/skills/deployment/SKILL.md) | Ship a build to production safely — pick a rollout strategy, gate on health, sequence migrations, and have a fast, tested rollback |
| [Incident Response](/skills/incident-response/SKILL.md) | Drive a production incident to resolution — stabilize first, communicate, find root cause, then write a blameless postmortem with follow-ups |
| [Webhooks](/skills/webhooks/SKILL.md) | Build reliable webhook senders and receivers — verify signatures, process idempotently, respond fast, and retry with backoff |
| [Auth](/skills/auth/SKILL.md) | Implement authentication and authorization safely — verify identity, manage sessions and tokens, store credentials securely, and enforce least-privilege access checks |

## Installation

### Option 1: Copy to your project (project-level)

Copy the `skills/` folder into your project under `.agents/skills/`:

```bash
mkdir -p /path/to/your/project/.agents/skills
cp -r skills/* /path/to/your/project/.agents/skills/
```

### Option 2: Install globally (all projects)

Copy the skill folders to your global Codex config:

```bash
cp -r skills/* ~/.codex/skills/
```

## Usage

Codex automatically discovers skills at startup. Once installed, you can ask Codex to use them:

```text
> generate a commit message for my staged changes
> write a PR description for this branch
> generate changelog for version 2.1.0
> review my branch changes for bugs and security issues
> generate tests for the files I changed
> refactor this function without changing its behavior
> write a Dockerfile for this project
> debug this stack trace and fix the root cause
> explain how the auth module works
> add docstrings to this file
> audit my dependencies for vulnerabilities
> profile this endpoint and make it faster
> design a REST API for managing orders
> run the linters and fix the issues
> add type annotations to this module
> scaffold a new settings component
> cut a new minor release
> write a migration to add an index to the orders table
> set up a GitHub Actions pipeline for this project
> audit this page for accessibility issues
> add structured logging to the payment flow
> find the commit that broke the login test
> write getting-started setup docs for new contributors
> write a README for this project
> scan the repo for hardcoded secrets and credentials
> figure out why this test is flaky and fix it
> find and remove dead code in this module
> harden the error handling in this function
> set up pre-commit hooks to lint and scan for secrets
> write a reproducible benchmark for this function
> put this new behavior behind a feature flag
> extract the hardcoded strings in this page for translation
> run tests only for the packages affected by my change
> add caching to this expensive query with proper invalidation
> add metrics and tracing to this service with alerts on errors
> add rate limiting to the login endpoint to stop brute-force attempts
> move these hardcoded settings into validated config with an example file
> plan a safe canary deploy for this release with a rollback path
> walk me through responding to this outage and draft a postmortem
> add a webhook receiver that verifies signatures and dedupes events
> add login with secure password hashing and per-request authorization checks
```

Codex will find the matching skill and follow its instructions.

## How Codex Skills Work

Each skill is a `SKILL.md` file inside a named folder with YAML frontmatter (`name` and `description`) and markdown instructions.

```text
skills/
  commit-message/
    SKILL.md
  pr-description/
    SKILL.md
  changelog/
    SKILL.md
  code-review/
    SKILL.md
  test-gen/
    SKILL.md
  refactor/
    SKILL.md
  dockerize/
    SKILL.md
  debug/
    SKILL.md
  explain/
    SKILL.md
  doc-gen/
    SKILL.md
  dependency-audit/
    SKILL.md
  optimize/
    SKILL.md
  api-design/
    SKILL.md
  lint-fix/
    SKILL.md
  type-annotate/
    SKILL.md
  scaffold/
    SKILL.md
  release/
    SKILL.md
  migrate/
    SKILL.md
  ci-setup/
    SKILL.md
  accessibility/
    SKILL.md
  logging/
    SKILL.md
  git-bisect/
    SKILL.md
  env-setup/
    SKILL.md
  readme/
    SKILL.md
  secrets-scan/
    SKILL.md
  flaky-test/
    SKILL.md
  dead-code/
    SKILL.md
  error-handling/
    SKILL.md
  pre-commit/
    SKILL.md
  benchmark/
    SKILL.md
  feature-flag/
    SKILL.md
  i18n/
    SKILL.md
  monorepo/
    SKILL.md
  caching/
    SKILL.md
  observability/
    SKILL.md
```

Key points:

- **name**: Short identifier for the skill
- **description**: Tells Codex when to use this skill (matched against user intent)
- **Body**: Step-by-step markdown instructions Codex follows
- **Discovery**: Codex scans `.agents/skills/` (project) and `~/.codex/skills/` (global) at startup

See [Codex Skills docs](https://developers.openai.com/codex/skills) for the full reference.
