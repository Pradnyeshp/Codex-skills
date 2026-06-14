---
name: caching
description: Add or improve caching safely — pick the right layer and key, set TTL and invalidation, and verify correctness and hit rate
---

# Caching

Speed up repeated work with a cache without serving stale or wrong data, using the project's existing infrastructure.

## Steps

1. **Confirm it's the right fix** — cache only proven-expensive, read-heavy, repeatable work (measure first; pair with the `optimize`/`benchmark` skills). Don't cache cheap or rarely-repeated calls
2. **Pick the layer** — choose the smallest cache that solves it: in-process memoization, a shared store (Redis/Memcached), HTTP/CDN caching, or DB query/result cache. Reuse the project's existing cache infrastructure rather than adding a new dependency
3. **Design the key** — make keys deterministic and fully scoped (include every input that changes the result: args, tenant/user, locale, version). Namespace keys to avoid collisions
4. **Set expiry** — choose a TTL that matches how fresh the data must be; prefer a bounded TTL over "cache forever". Add a max size / eviction policy for in-memory caches to bound memory
5. **Plan invalidation** — decide how stale entries get cleared on writes (explicit bust, versioned keys, or short TTL). Invalidation is the hard part — make it explicit, not hopeful
6. **Handle misses and failures** — cache lookups must fall back to the source on miss or cache outage; never let a cache error break the request. Consider stampede protection (single-flight/locking) for hot keys
7. **Verify** — confirm correct values on hit and miss, that invalidation actually clears entries, and measure the hit rate / latency improvement to prove it helps

## Rules

- **Measure before caching** — don't add a cache to code that isn't a proven, repeated bottleneck
- Keys must be **deterministic and fully scoped**; a key missing an input that affects the result serves wrong data across users/tenants/locales
- Prefer **bounded TTL + explicit invalidation** over indefinite caching; "cache forever" is a stale-data bug waiting to happen
- A cache miss or cache-store outage must **fall back to the source**, never error out
- **Bound memory** for in-process caches (max size + eviction); an unbounded cache is a memory leak
- Don't cache **user-specific or sensitive data** in a shared/global scope — segment by identity and respect privacy
- Protect hot keys from **cache stampedes** (single-flight or locking) when recomputation is expensive
- Reuse the project's existing cache layer and conventions; don't introduce a new caching system without reason
- Verify invalidation works — an untested bust path is the most common caching bug
