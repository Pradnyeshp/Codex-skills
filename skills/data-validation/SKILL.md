---
name: data-validation
description: Validate and sanitize input at trust boundaries — define a schema, fail closed with clear errors, normalize data, and never trust client-supplied values
---

# Data Validation

Validate untrusted input at the edges of the system (HTTP requests, queue messages, file uploads, third-party responses) using the project's existing validation library (e.g. Zod, Pydantic, JSON Schema, ActiveModel, Joi), so bad data is rejected before it reaches business logic.

## Where and how

1. **Validate at the boundary** — check input as it enters the system, before it touches business logic or the database. Past the boundary, code should be able to trust the data's shape
2. **Define a schema, don't hand-roll checks** — declare the expected types, formats, ranges, and required fields in one schema and validate against it. Centralized schemas stay consistent and double as documentation
3. **Fail closed and reject by default** — accept only what matches; reject unexpected or extra fields rather than ignoring them, so attackers can't smuggle in values (mass-assignment / over-posting)
4. **Validate the parsed value, not just the type** — enforce ranges, lengths, enums, formats, and cross-field rules (e.g. `start < end`), not merely "is a string"
5. **Normalize after validating** — trim, lowercase emails, canonicalize, coerce to the right type, so downstream code sees consistent data

## Errors and safety

1. **Return clear, actionable errors** — say which field failed and why, with a `400`/`422`; collect all errors rather than stopping at the first when it helps the caller
2. **Don't leak internals** — error messages should guide the caller, not expose stack traces, SQL, or internal field names
3. **Set bounds to resist abuse** — cap string lengths, array sizes, number ranges, and payload size so a huge or deeply-nested input can't exhaust memory; pair with the `rate-limiting` skill
4. **Validation is not output encoding** — also escape/parameterize at the sink (SQL, HTML, shell) to prevent injection; validation reduces but doesn't replace it
5. **Re-validate at every trust boundary** — data from another internal service or the DB that originated from a user is still untrusted; don't assume it was checked upstream

## Rules

- **Validate untrusted input at the boundary**, before business logic — never trust client-supplied values, including ids, roles, and prices
- **Use a declarative schema** as the single source of truth, not scattered ad-hoc `if` checks
- **Reject unknown/extra fields** (allowlist, not denylist) to prevent mass-assignment
- **Check values, not just types** — ranges, lengths, formats, enums, and cross-field invariants
- **Fail closed with a clear `400`/`422`** naming the offending field, without leaking internals
- **Bound sizes and depth** (string length, array count, payload size) to resist resource-exhaustion; pair with `rate-limiting`
- **Normalize after validation** so downstream code sees canonical, consistent data
- **Still encode/parameterize at the sink** — validation complements, but doesn't replace, injection defenses
- **Re-validate across internal trust boundaries**; don't assume an upstream service already did
- **Test the invalid cases**, not just the happy path — pair with `test-gen`
