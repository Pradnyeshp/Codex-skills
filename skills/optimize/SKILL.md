---
name: optimize
description: Profile a slow function, endpoint, or query, find the real bottleneck with measurements, and apply a verified performance fix
---

# Optimize

Improve performance based on measurements — never on guesses.

## Steps

1. **Define the target** — get the specific slow operation (function, endpoint, query, page load) and a concrete goal or baseline. Ask if not provided
2. **Measure first** — capture a baseline with a real tool or timing harness before changing anything:
   - Profilers: `cProfile`/`py-spy` (Python), `node --prof`/`clinic` (Node), `pprof` (Go), `perf`/`cargo flamegraph` (Rust)
   - Timing: a focused benchmark, `time`, or an in-code timer around the hot path
3. **Find the bottleneck** — identify where the time or memory actually goes (the top frames, the N+1 query, the allocation hotspot). State it in one sentence
4. **Form a hypothesis** — name the single change most likely to help and why (algorithmic complexity, redundant work, I/O, allocation, caching)
5. **Apply the smallest change** that addresses the bottleneck, preserving behavior
6. **Re-measure** — run the same benchmark and report before/after numbers. Keep the change only if the improvement is real
7. **Confirm correctness** — run the existing tests to ensure no regression

## Rules

- Measure before and after — report concrete numbers, not impressions
- Optimize the **proven** bottleneck, not the one that looks slow
- Prefer algorithmic and I/O wins (complexity, N+1 queries, batching, caching) over micro-optimizations
- Do not sacrifice readability for marginal gains; note the tradeoff when you do
- Keep behavior identical — a faster wrong answer is still wrong
- If the bottleneck is external (network, third-party API, hardware), say so and suggest a strategy rather than forcing a code change
