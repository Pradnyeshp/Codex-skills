---
name: idempotency
description: Make write operations safe to retry — accept idempotency keys, dedupe on a stored result, handle concurrent retries, and keep effects exactly-once
---

# Idempotency

Make state-changing operations (payments, orders, sends) safe to retry so a duplicate request — from a client retry, a network timeout, or an at-least-once queue — produces one effect, not many, using the project's existing storage.

## How it works

1. **Accept an idempotency key** — the client sends a unique key (e.g. `Idempotency-Key` header or a field) per logical operation; retries of the *same* operation reuse the *same* key
2. **Store the key with the result** — on first request, record the key, the request fingerprint, and the response; on a repeat, return the stored response instead of re-running the effect
3. **Reserve the key atomically** — insert the key with a unique constraint *before* doing the work, so two concurrent requests with the same key can't both proceed (one wins, the other waits or returns the in-progress/stored result)
4. **Scope the key correctly** — per endpoint and per account/tenant, so unrelated operations can't collide and one user's key can't replay another's

## Edge cases

1. **Detect key reuse with a different payload** — if the same key arrives with a different request body, reject with a 4xx rather than silently returning the old result; this catches client bugs
2. **Handle the in-flight case** — if a retry arrives while the first is still processing, don't double-execute; return a "processing" status or block on the reservation
3. **Expire stored keys** — keep results long enough to cover realistic retries (hours/days) then purge; don't store forever
4. **Make the effect itself idempotent where possible** — use natural keys / unique constraints at the data layer (e.g. unique order id) as a second line of defense, not just the key table
5. **Wrap reservation + effect + result in one transaction** (or an outbox) so a crash can't leave the key recorded without the effect, or vice versa

## Rules

- **Reserve the idempotency key atomically before the effect** — a unique constraint is what actually prevents the double-write under concurrency
- **Return the stored response on replay** instead of re-executing — the second call must produce no new effect
- **Bind the key to the request payload** and reject reuse with a mismatched body as a 4xx
- **Scope keys per endpoint and per account** so they can't collide or be replayed across users
- **Handle in-flight retries** explicitly — never let a concurrent duplicate run the effect twice
- **Back idempotency with data-layer unique constraints** (natural keys) as defense in depth
- **Expire keys** after a sensible retention window; don't keep them indefinitely
- **Make effect + key + result atomic** (transaction or outbox) so partial failures don't desync them
- Pair with `background-jobs` and `webhooks` (at-least-once delivery makes idempotency mandatory) and `data-validation` for the key/payload checks
