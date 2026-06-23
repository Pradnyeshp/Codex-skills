---
name: webhooks
description: Build reliable webhook senders and receivers — verify signatures, process idempotently, respond fast, and retry with backoff
---

# Webhooks

Send or receive webhooks reliably and securely, so events aren't lost, duplicated, or spoofed, using the project's existing HTTP and queue infrastructure.

## Receiving

1. **Verify authenticity first** — validate the signature (HMAC over the raw body with the shared secret) before parsing or trusting anything; reject bad signatures with 401. Use the **raw** request body for the check — re-serializing changes the bytes and breaks the signature
2. **Guard against replay** — check the timestamp is recent and reject stale requests; this stops captured payloads from being replayed
3. **Respond fast, process async** — acknowledge with 2xx immediately and hand the work to a queue/background job. Doing heavy work inline risks a sender timeout and a redelivery storm
4. **Process idempotently** — dedupe on the event/delivery id; the same event will arrive more than once, so handling must be safe to repeat without double effects
5. **Return honest status codes** — 2xx only when you've safely accepted the event; 4xx for permanently bad requests (don't make the sender retry forever); 5xx when you want a retry

## Sending

1. **Sign every payload** — HMAC the body with a per-subscriber secret and include the signature + timestamp in headers so receivers can verify
2. **Retry with backoff** — on non-2xx/timeout, retry with exponential backoff and jitter up to a cap; give each delivery a stable id so receivers can dedupe
3. **Send a dead-letter / give up cleanly** — after max retries, stop and record the failure for inspection/replay rather than retrying forever
4. **Make deliveries observable** — log attempts, status, and latency; expose delivery history. Pair with the `observability` skill

## Rules

- **Verify the signature before doing anything else**, using the raw body — an unverified webhook is untrusted input and a spoofing vector
- **Reject stale timestamps** to prevent replay of captured payloads
- **Acknowledge fast, work async** — never run slow processing inline in the request; it causes sender timeouts and duplicate deliveries
- **Always dedupe** on the delivery/event id — at-least-once delivery means duplicates are normal, not exceptional
- Use **correct status codes**: 2xx = accepted, 4xx = don't retry, 5xx = retry; lying causes lost events or infinite retries
- Senders must **retry with capped exponential backoff + jitter** and then dead-letter — neither give up on the first failure nor hammer forever
- **Never put secrets in the URL or payload**, and store webhook secrets like any other credential; pair with `secrets-scan`
- Apply **rate limiting / size limits** on receive endpoints so a misbehaving sender can't overwhelm you; pair with `rate-limiting`
- Make every delivery **traceable** (delivery id + attempt log) so failures can be diagnosed and replayed
