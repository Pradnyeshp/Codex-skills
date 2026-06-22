---
name: deployment
description: Ship a build to production safely — pick a rollout strategy, gate on health, sequence migrations, and have a fast, tested rollback
---

# Deployment

Get a release into production with minimal risk and a fast way back out, using the project's existing deploy tooling and platform.

## Steps

1. **Understand the target** — find how the project already deploys (platform, pipeline, environments, who triggers it) and reuse it. Identify what changed in this release and its blast radius
2. **Pick a rollout strategy** — match risk to traffic: **rolling** (gradual instance replacement, simple), **blue-green** (instant switch, easy rollback, double resources), or **canary** (small % first, watch, then ramp). Default to the smallest safe increment for risky changes
3. **Sequence schema changes safely** — make migrations backward-compatible so old and new code run together during rollout (expand-then-contract: add columns/tables first, deploy code, backfill, drop later). Pair with the `migrate` skill; never deploy a destructive migration in the same step as the code that needs it
4. **Gate on health** — wire readiness/liveness checks and key metrics into the rollout so a bad version is caught before full traffic. Pair with the `observability` skill to watch error rate and latency during the ramp
5. **Define the rollback** — know exactly how to revert (previous image/tag, traffic switch, or feature-flag kill) before you ship. A rollback that's never been tested is a hope, not a plan
6. **Decouple deploy from release** — ship code dark behind a flag and turn it on separately so a deploy and a feature launch can be rolled back independently. Pair with the `feature-flag` skill
7. **Communicate and time it** — announce the deploy, avoid risky windows, and have an owner watching. Record what shipped (commit/tag) for traceability
8. **Verify** — confirm the new version is serving, health checks pass, key metrics are steady through the ramp, and a rollback actually restores the prior version in a test

## Rules

- **Always have a tested rollback** before shipping — know the exact revert step and confirm it works, don't discover it during an incident
- Make migrations **backward-compatible** and decoupled from the code that needs them (expand/contract); old and new versions run together mid-rollout
- **Gate the rollout on health** (readiness checks + error/latency metrics); promote only when the new version is proven healthy, not on a timer alone
- **Roll out incrementally** for risky changes (canary/blue-green); don't flip 100% of traffic to an unverified build
- **Decouple deploy from release** — ship dark behind a flag so launching a feature and reverting a deploy are independent actions
- **Never run destructive/irreversible migrations** in lockstep with the deploy; separate, backfill, and verify before dropping anything
- Reuse the project's **existing deploy pipeline and platform**; don't hand-deploy around the established process
- Keep deploys **traceable** — record the exact commit/tag/image shipped so any version is reproducible and revertible
- **Watch the deploy** — an unobserved rollout hides the failure it was meant to catch; have an owner and live metrics
