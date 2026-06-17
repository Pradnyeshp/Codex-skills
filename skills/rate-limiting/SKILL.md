---
name: rate-limiting
description: Protect a service with rate limiting and throttling — pick the algorithm and scope, set limits, return correct headers, and fail safely
---

# Rate Limiting

Protect a service from abuse and overload by capping request rates fairly, without breaking legitimate traffic, using the project's existing infrastructure.

## Steps

1. **Identify what to protect** — find the expensive, abusable, or scarce paths (login/auth, writes, search, third-party-backed calls). Don't blanket-limit cheap idempotent reads
2. **Choose the scope (key)** — decide what a limit counts against: API key, user id, IP, or tenant. Prefer authenticated identity over IP (IPs are shared/spoofable); namespace keys per route class where limits differ
3. **Pick the algorithm** — match the traffic shape: **token bucket** (allows bursts, smooth refill), **sliding window** (accurate, steadier), or **fixed window** (simplest, but boundary bursts). Reuse the project's existing limiter/gateway rather than adding a new one
4. **Set the limits** — base them on real capacity and usage data, not guesses; allow a sensible burst above the steady rate. Make limits configurable, not hardcoded
5. **Use a shared store for distributed services** — counters must live in a shared store (Redis/gateway) so the limit holds across instances; per-process counters silently multiply the real limit by the instance count
6. **Respond correctly** — on limit, return **429 Too Many Requests** with `Retry-After`; include `RateLimit-Limit`/`RateLimit-Remaining`/`RateLimit-Reset` headers so clients can back off. Keep the rejection cheap
7. **Fail open or closed deliberately** — decide what happens when the limiter store is down: fail **open** (allow) for availability, **closed** (deny) for protection-critical paths like auth. Make the choice explicit
8. **Verify** — exceed the limit in a test and confirm 429 + headers, confirm the limit resets, confirm it holds across multiple instances, and confirm legitimate burst traffic isn't wrongly throttled

## Rules

- Limit the **expensive and abusable** paths first; don't throttle cheap reads that don't need it
- Prefer **authenticated identity** (API key/user/tenant) as the key over raw IP — IPs are shared behind NAT/proxies and spoofable
- In a multi-instance deployment, counters **must** be in a shared store; per-process counters multiply the effective limit by the number of instances
- Always return **429** with `Retry-After` and `RateLimit-*` headers — let clients back off gracefully instead of hammering
- Trust the **real client** behind proxies (validated `X-Forwarded-For`/proxy headers), not a header any caller can forge
- Decide **fail-open vs fail-closed** explicitly for limiter outages; don't let a dead Redis silently disable all protection
- Allow a **burst** above the steady rate so normal bursty clients aren't punished; tune from real usage data
- Make limits **configurable** per route/tier and easy to change without a redeploy
- Keep the limiter **low-overhead and resilient** — a check on every request must be fast and must not take down the service if it fails
- Pair with `observability` to track throttle rates and with `caching` to reduce load before limiting it
