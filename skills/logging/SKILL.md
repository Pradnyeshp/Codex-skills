---
name: logging
description: Add structured, leveled logging and basic observability to code using the project's logging library and conventions
---

# Logging

Add useful, structured logging that aids debugging in production without adding noise.

## Steps

1. **Identify the scope** — the module or flow that needs better observability, and what problem it's meant to help diagnose
2. **Detect the logging setup** — find the existing logger and convention (e.g. `logging` (Python), `winston`/`pino` (Node), `slog`/`zap` (Go), `tracing`/`log` (Rust)). Reuse it; don't introduce a new one
3. **Place logs at the right boundaries** — entry/exit of significant operations, external calls, state transitions, and error paths. Avoid logging inside tight loops or hot paths
4. **Use appropriate levels** — `error` for failures needing attention, `warn` for recoverable anomalies, `info` for key business events, `debug` for diagnostic detail
5. **Make logs structured** — attach contextual fields (request/trace id, user id, operation, duration) as key-value pairs rather than interpolating everything into the message
6. **Protect sensitive data** — never log secrets, passwords, tokens, or PII; redact or omit
7. **Verify** the code runs and the logs are emitted at the expected levels and format

## Rules

- Reuse the project's logger and format — consistency matters more than personal preference
- Log at boundaries and decisions, not every line; signal over noise
- **Never log secrets, credentials, tokens, or PII** — redact them
- Prefer structured key-value context over string interpolation so logs are queryable
- Choose levels deliberately so production can filter; don't log everything at `info`
- Include correlation context (request/trace id) where the framework provides it
- Don't change program behavior — logging is observation, not control flow
