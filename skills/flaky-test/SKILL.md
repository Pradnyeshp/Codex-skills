---
name: flaky-test
description: Diagnose and stabilize a flaky or intermittently failing test by finding the real source of nondeterminism
---

# Flaky Test

Make an intermittent test deterministic by fixing the actual cause, not by masking it.

## Steps

1. **Reproduce the flake** — run the test repeatedly (and in isolation vs. with the full suite) to confirm it really is intermittent and capture a failing run
2. **Identify the nondeterminism source** — look for the usual culprits: time/date and timezone, randomness without a seed, unordered collections, concurrency/async races, shared mutable state or test ordering, network/external calls, and resource cleanup between tests
3. **Narrow it down** — vary one factor at a time (run order, seed, parallelism, clock) to confirm which input flips the result
4. **Apply a deterministic fix** — pin or inject the clock, seed the RNG, await/synchronize properly, sort before asserting, isolate state with proper setup/teardown, and stub external dependencies
5. **Verify** — run the test many times (e.g. dozens of loops) in isolation and within the suite to confirm it now passes consistently
6. **Prevent recurrence** — note the pattern and check for the same issue in sibling tests

## Rules

- Fix the **root cause** of nondeterminism — never paper over it with retries, `sleep`, or by deleting/skipping the assertion
- A test that "passes now" after one run is not fixed; demonstrate stability by running it many times
- Control time and randomness explicitly (inject a clock, seed the RNG) rather than asserting on live values
- Don't rely on dict/map/set ordering or filesystem ordering — sort before comparing
- Make tests independent of run order: no shared mutable state leaking across tests
- Replace real network/external calls with stubs or fakes so the test doesn't depend on the outside world
- Adding a retry wrapper is a last resort and must be justified; flag it as a known compromise, not a fix
