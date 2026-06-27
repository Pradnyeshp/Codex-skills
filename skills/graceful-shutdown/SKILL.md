---
name: graceful-shutdown
description: Shut a service down cleanly — catch termination signals, stop taking new work, drain in-flight requests and jobs, close resources, and exit within the deadline
---

# Graceful Shutdown

Make a service exit cleanly on deploy, scale-down, or restart so in-flight work finishes (or is safely requeued) and no requests are dropped, using the project's existing server/worker lifecycle hooks.

## Shutdown sequence

1. **Catch termination signals** — handle `SIGTERM` (and `SIGINT` in dev); orchestrators send `SIGTERM`, wait a grace period, then `SIGKILL`. Your job is to finish before the kill
2. **Flip to "not ready" first** — fail the readiness probe / deregister from the load balancer so new traffic stops being routed to this instance. Keep liveness passing so you aren't killed early
3. **Stop accepting new work** — close the listener to new connections, and stop pulling new jobs from the queue, while letting current work continue
4. **Drain in-flight work** — wait for active requests/jobs to finish, bounded by a timeout. For long jobs that won't finish, requeue them so another worker picks them up. Pair with the `background-jobs` skill
5. **Close resources in order** — flush logs/metrics/telemetry, then close DB pools, queue connections, and file handles. Release locks
6. **Exit with the right code** — `0` on a clean drain; non-zero if forced past the deadline, so it's visible

## Getting the timing right

1. **Set the drain timeout below the orchestrator's grace period** — if Kubernetes `terminationGracePeriodSeconds` is 30s, finish draining in ~25s, leaving room to exit before `SIGKILL`
2. **Account for LB propagation** — there's a delay between failing readiness and traffic actually stopping; a short sleep after marking not-ready avoids dropping requests already in flight
3. **Make shutdown idempotent** — a second signal during shutdown should be a no-op (or force-exit), not restart the sequence

## Rules

- **Trap `SIGTERM` and run an ordered shutdown** — never rely on the default behavior, which drops in-flight work
- **Mark not-ready / deregister before draining** so the load balancer stops sending new requests first
- **Stop accepting new work, then drain existing work** with a bounded timeout — don't wait forever, but don't cut active work off instantly
- **Keep the drain deadline under the orchestrator's grace period** so you exit cleanly before `SIGKILL`
- **Requeue jobs that can't finish in time** instead of losing them; pair with `background-jobs`
- **Close resources in dependency order** and flush telemetry before exit so no data is lost
- **Make the handler idempotent** — repeated signals must not restart or corrupt the sequence
- **Exit non-zero when forced past the deadline** so failed drains are visible; pair with `observability`
- **Verify it** — send `SIGTERM` under load and confirm zero dropped requests and no orphaned/lost jobs
