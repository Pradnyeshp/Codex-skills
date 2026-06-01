---
name: api-design
description: Design a REST or GraphQL API endpoint or resource — paths, methods, schemas, status codes, and errors — following the project's conventions
---

# API Design

Design a clear, consistent API surface before implementing it.

## Steps

1. **Understand the need** — get the resource(s), the operations required, and who consumes the API. Ask for missing context
2. **Survey existing conventions** — read neighboring endpoints/handlers to match naming, versioning, auth, pagination, and error format. Consistency beats personal preference
3. **Model the resources** — define each resource's fields, types, required/optional, and relationships
4. **Define the operations** — for each: method/path (REST) or query/mutation (GraphQL), request body/params, success response shape, and status codes
5. **Specify errors** — list failure cases with status codes and a consistent error body
6. **Cover the cross-cutting concerns** — auth/permissions, validation rules, pagination/filtering/sorting, idempotency, and rate limits where relevant
7. **Present the contract** for review before writing implementation code

## Output Format

```text
## <Resource> API

### Endpoints
- `GET /v1/things` — list (query: `?limit`, `?cursor`) → 200 `{ data: Thing[], next_cursor }`
- `POST /v1/things` — create → 201 `Thing` | 400 validation | 409 conflict
- `GET /v1/things/{id}` — fetch → 200 `Thing` | 404
- `PATCH /v1/things/{id}` — update → 200 `Thing` | 404 | 422

### Schema: Thing
- `id` string (uuid, read-only)
- `name` string (required, 1–100 chars)
- `created_at` string (iso-8601, read-only)

### Errors
`{ "error": { "code": string, "message": string, "details"?: object } }`
```

## Rules

- Follow REST/GraphQL conventions: correct verbs/methods, plural nouns, proper status codes (200/201/204/400/401/403/404/409/422)
- Match the existing project style for naming, casing, versioning, and error envelopes
- Make responses predictable: stable field names, explicit types, no leaking internal models
- Design for evolution — version the surface and prefer additive changes
- Call out auth, validation, and pagination explicitly; don't leave them implicit
- Present the contract for approval first; implement only when the user asks
