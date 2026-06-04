---
name: error-handling
description: Harden error handling and edge cases — validate inputs, handle failures explicitly, and fail safely without leaking internals
---

# Error Handling

Make code robust against bad input and failing dependencies, following the language's idioms.

## Steps

1. **Map the failure surface** — identify where things can go wrong: external calls (network, DB, filesystem), parsing/deserialization, user input, arithmetic/bounds, null/None, and resource acquisition
2. **Check current handling** — find swallowed exceptions, bare `catch`/`except`, ignored error returns, missing validation, and unhandled edge cases (empty, null, large, negative, malformed)
3. **Validate inputs at the boundary** — reject or normalize bad input early, with clear messages, before it propagates deeper
4. **Handle failures explicitly** — catch what you can act on, let the rest propagate; never silently swallow errors. Use the language idiom (exceptions, `Result`/`Either`, error returns) consistently with the codebase
5. **Fail safely** — clean up resources (use `finally`/`with`/`defer`/RAII), preserve invariants, and avoid leaving partial state. Add retries/timeouts only where transient failures are expected
6. **Surface useful, safe errors** — actionable messages and proper log context, without leaking secrets, stack traces, or internals to end users
7. **Verify** — add or run tests for the new edge cases and confirm existing tests still pass

## Rules

- Never silently swallow errors — no empty `catch`/`except`, no ignored error returns; handle, wrap with context, or propagate
- Validate at trust boundaries (user input, external data) and fail fast with clear messages
- Catch specific, actionable errors rather than blanket-catching everything; don't catch what you can't handle
- Always release resources on every path (files, connections, locks) using the language's cleanup idiom
- Don't leak sensitive data or internal details in error messages shown to users; log details separately
- Preserve the original error/cause when wrapping so debugging context isn't lost
- Match the codebase's existing error-handling style and types rather than introducing a new pattern
- Add retries/timeouts only for genuinely transient failures, with bounds and backoff — not as a blanket wrapper
