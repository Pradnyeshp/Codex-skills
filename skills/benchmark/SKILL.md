---
name: benchmark
description: Write and run reproducible performance benchmarks with stable, statistically meaningful measurements using the project's tooling
---

# Benchmark

Measure performance reliably so changes can be compared with confidence — distinct from optimizing, which acts on the results.

## Steps

1. **Define the question** — decide exactly what to measure (a function, endpoint, query, end-to-end path) and the metric that matters (latency, throughput, allocations, memory)
2. **Pick the harness** — use the ecosystem's standard tool: `go test -bench`/`benchstat`, `pytest-benchmark`, JMH (Java), `criterion`/`cargo bench` (Rust), `benchmark.js`/`vitest bench` (JS), `hyperfine` for CLIs. Don't hand-roll timing if a real harness exists
3. **Build a representative workload** — realistic input sizes and data distributions; include warmup to exclude cold-start/JIT effects, and isolate the code under test from setup
4. **Control the environment** — fix versions and inputs, disable noisy neighbors where possible, pin iterations/duration, and run enough samples for a stable result
5. **Run and capture** — record a baseline before the change and the candidate after, with the same harness and machine; report median/percentiles and variance, not a single run
6. **Compare honestly** — use a comparison tool (e.g. `benchstat`) or report deltas with spread; call out whether the difference exceeds the noise
7. **Make it repeatable** — commit the benchmark so others can reproduce it, and document how to run it

## Rules

- Always **warm up** and discard cold iterations; measure steady state unless cold start is the point
- Report **distribution** (median + p95/variance), never a single timing — one run is not a measurement
- Keep inputs and environment **fixed** between baseline and candidate; change one thing at a time
- Use the language's real benchmark harness rather than ad-hoc `time` calls when one exists
- Beware dead-code elimination — consume results so the compiler can't optimize the work away
- Don't benchmark on a loaded machine or in CI noise and present it as authoritative; note the environment
- State the baseline explicitly and whether a delta is within noise — don't claim a speedup you can't distinguish from variance
- Keep the benchmark committed and runnable so results can be reproduced later
