---
name: incident-response
description: Drive a production incident to resolution — stabilize first, communicate, find root cause, then write a blameless postmortem with follow-ups
---

# Incident Response

Handle a production incident calmly and methodically — restore service first, then understand why — using the project's existing alerting, runbooks, and tooling.

## Steps

1. **Assess and declare** — confirm it's real and gauge impact (who/what is affected, how badly). Assign a severity and, for anything user-facing, declare an incident with a single owner/coordinator so response isn't chaotic
2. **Stabilize before diagnosing** — the first goal is to stop the bleeding, not to find the root cause. Reach for the fastest safe mitigation: roll back the recent deploy, flip a feature flag off, scale up, or shed load. Pair with the `deployment` skill for rollback
3. **Mitigate with the lowest-risk action** — prefer a known-good revert over a speculative forward fix under pressure; don't make risky changes to a system you don't yet understand mid-incident
4. **Communicate on a cadence** — post a clear status (impact, what's being done, next update time) at a regular interval to the expected channel. Keep updates factual; over-communicate rather than go silent
5. **Gather evidence as you go** — capture the timeline, metrics, logs, and what changed (recent deploys/config). Pair with the `observability` skill. This both speeds diagnosis and feeds the postmortem
6. **Confirm recovery** — verify metrics are back to normal and the symptom is actually gone, not just quiet. Watch for a while before declaring resolved; announce all-clear explicitly
7. **Write a blameless postmortem** — within a few days, document timeline, impact, root cause (the system/process failure, not a person), what went well, and what didn't. Pair with the `git-bisect` and `debug` skills to pin the true cause
8. **Track follow-ups to done** — turn the postmortem into concrete, owned, prioritized action items (detection gaps, missing safeguards, runbook fixes) and ensure they're actually completed, not filed and forgotten

## Rules

- **Stabilize before diagnosing** — restore service with the fastest safe mitigation first; root-cause analysis comes after the bleeding stops
- Prefer a **known-good rollback** over a speculative forward fix under pressure; don't experiment on a system you don't fully understand mid-incident
- **One owner/coordinator** per incident — diffuse ownership means things get dropped or done twice
- **Communicate on a regular cadence**, including "still investigating"; silence during an outage erodes trust faster than bad news
- **Verify real recovery** against metrics before standing down — "looks quiet" is not "fixed"
- Keep postmortems **blameless** — target the system and process gaps that let it happen, never an individual; blame kills the honesty that prevents recurrence
- Capture the **timeline and evidence during** the incident, not from memory afterward
- Every postmortem produces **owned, tracked follow-ups**; an action item nobody owns or finishes guarantees the same incident returns
- Match the project's **existing severity levels, runbooks, and comms channels**; an incident is the wrong time to invent process
