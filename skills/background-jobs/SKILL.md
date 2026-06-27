---
name: background-jobs
description: Run work asynchronously with a job queue — enqueue reliably, process idempotently, retry with backoff, dead-letter failures, and make jobs observable
---

# Background Jobs

Move slow or unreliable work out of the request path into a queue/worker, using the project's existing job framework (e.g. Sidekiq, Celery, BullMQ, RQ, Resque, a cloud queue), so requests stay fast and work isn't lost.

## Designing the job

1. **Enqueue small, serializable arguments** — pass an id, not a whole object. The record may change between enqueue and run, so re-fetch fresh state inside the job
2. **Make every job idempotent** — at-least-once delivery means a job can run more than once (retries, redeliveries, crashes). Dedupe on a job/business key so a repeat run is safe and doesn't double-charge or double-send
3. **Keep jobs small and single-purpose** — one unit of work per job; fan large work out into many jobs rather than one giant long-running task
4. **Set explicit timeouts** so a stuck job can't hold a worker forever

## Reliability

1. **Retry transient failures with capped exponential backoff + jitter** — network blips and rate limits are temporary; don't retry instantly or forever
2. **Don't retry permanent failures** — bad input or a 4xx won't succeed on retry; fail fast and dead-letter it
3. **Dead-letter after max attempts** — move exhausted jobs to a dead-letter queue with the error and payload for inspection and replay, instead of dropping them silently
4. **Mind enqueue/commit ordering** — if you enqueue inside a DB transaction, the job can start before the commit lands (or run after a rollback). Enqueue after commit, or use a transactional outbox
5. **Choose the right delivery semantics** — most queues are at-least-once; rely on idempotency rather than assuming exactly-once

## Operating

1. **Separate queues by priority/latency** — keep fast, user-facing jobs off the same queue as slow bulk work so one doesn't starve the other
2. **Make jobs observable** — log job id, attempt, and duration; emit metrics for queue depth, age of oldest job, and failure rate; alert on a growing backlog. Pair with the `observability` skill
3. **Handle worker shutdown gracefully** — stop pulling new jobs, finish or requeue in-flight ones on SIGTERM. Pair with the `graceful-shutdown` skill

## Rules

- **Enqueue ids, not snapshots** — re-fetch current state inside the job so it acts on fresh data
- **Make jobs idempotent and dedupe on a key** — at-least-once delivery makes duplicate runs normal, not exceptional
- **Retry transient errors with capped backoff + jitter; never retry permanent ones** — and dead-letter after the cap instead of dropping work
- **Enqueue after the transaction commits** (or use an outbox) so jobs never run against uncommitted or rolled-back data
- **Set per-job timeouts** so one stuck job can't tie up a worker indefinitely
- **Don't lose failures** — route exhausted jobs to a dead-letter queue with enough context to diagnose and replay
- **Isolate queues by priority** so bulk work can't starve latency-sensitive jobs
- Make jobs **observable** (id, attempt, duration, queue depth) and **alert on backlog**; pair with `observability`
- **Drain cleanly on shutdown** — finish or requeue in-flight jobs; pair with `graceful-shutdown`
- **Never log secrets or full payloads** with sensitive data; pair with `secrets-scan`
