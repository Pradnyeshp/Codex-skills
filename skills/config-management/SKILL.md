---
name: config-management
description: Structure application configuration safely — layer sources, validate at startup, keep secrets out of code, and document every setting
---

# Config Management

Make an app's configuration explicit, validated, and environment-portable — so it runs the same way everywhere and fails fast on misconfiguration, using the project's existing config approach.

## Steps

1. **Inventory the settings** — find what's currently configured and what's hardcoded (URLs, timeouts, feature toggles, credentials, pool sizes). Hardcoded environment-specific values are config that hasn't been extracted yet
2. **Separate config from secrets** — plain config (timeouts, hostnames, flags) can live in committed files; secrets (keys, passwords, tokens) come from the environment or a secrets manager and are never committed. Pair with the `secrets-scan` skill to catch leaks
3. **Define precedence** — establish one clear order, typically defaults < config file < environment variables < explicit overrides. Document it so the effective value is always predictable
4. **Validate at startup** — parse and check all config once on boot: required keys present, correct types, values in range, URLs well-formed. Fail fast with a clear message naming the bad key rather than crashing deep in a request
5. **Provide safe defaults** — sensible defaults for non-secret settings so local dev works out of the box; never default a secret or a production-only value to something insecure
6. **Centralize access** — load config into one typed object/module read at startup; don't scatter raw `getenv` calls across the codebase. This makes settings discoverable and testable
7. **Document every setting** — keep an up-to-date example file (`.env.example`/config template) listing each key, what it does, its default, and whether it's required. Pair with the `env-setup` skill for onboarding
8. **Verify** — boot with valid config and confirm it loads; boot with a missing/invalid required key and confirm it fails fast with a useful error; confirm no secret values are committed

## Rules

- **Never commit secrets** — keys, passwords, and tokens come from the environment or a secrets manager, not source files; pair with `secrets-scan`
- **Validate on startup, fail fast** — a misconfigured app should refuse to boot with a clear message, not fail mysteriously mid-request
- Keep **one documented precedence order**; the effective value of any setting must be predictable, not source-dependent
- Provide **safe non-secret defaults** so local dev works without setup; never default a secret or weaken security to make something "just work"
- **Centralize config access** into one loaded, typed object; scattered raw env reads are untestable and hide what's configurable
- Keep an **example/template file** in sync with the real keys — an undocumented setting is one nobody knows to set
- **Don't log full config** — it leaks secrets; log only non-sensitive, explicitly-allowed values, redacting the rest
- Match the project's **existing config library and conventions**; don't introduce a second config system
- Treat **environment-specific hardcoded values as config to extract**, not as constants to leave in place
