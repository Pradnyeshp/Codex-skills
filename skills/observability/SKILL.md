---
name: observability
description: Add metrics, tracing, and actionable alerts so a service's health and behavior are visible in production, using the project's stack
---

# Observability

Make a service observable — surface metrics, traces, and alerts that answer "is it healthy and why" — complementing structured logging.

## Steps

1. **Detect the stack** — find what's already in use (Prometheus/OpenTelemetry/StatsD, Grafana, Datadog, Sentry, Jaeger). Reuse it and its conventions rather than adding a parallel system. Pairs with the `logging` skill for the logs pillar
2. **Instrument the key metrics** — cover the RED/USE basics for the critical paths: request **rate**, **errors**, and **duration** (latency percentiles), plus saturation of key resources. Use consistent metric names and units
3. **Add tracing on critical paths** — propagate trace/span context across service and external-call boundaries so a slow request can be followed end to end; record key attributes (route, status, downstream calls)
4. **Use low-cardinality labels** — tag metrics by route/status/method, never by unbounded values (user id, request id, raw URL) — high cardinality blows up the metrics backend
5. **Expose health checks** — provide liveness/readiness endpoints and a metrics scrape/export endpoint as the platform expects
6. **Define actionable alerts** — alert on user-visible symptoms (error rate, latency SLO burn, saturation), not raw internals; give each alert a threshold, duration, and a clear "what to do" runbook pointer
7. **Verify** — generate traffic (including errors) and confirm metrics increment, traces appear and connect across boundaries, and an alert would fire on the simulated condition

## Rules

- Instrument the **symptoms users feel** (rate, errors, latency, saturation) before deep internal counters
- Keep metric **labels low-cardinality** — never label by user id, request id, or raw path; that overwhelms the backend
- **Propagate trace context** across service and external boundaries, or traces become disconnected and useless
- Every alert must be **actionable**: a clear threshold, a duration to avoid flapping, and a runbook/next step — no alerts nobody acts on
- Alert on **symptoms/SLOs**, not on every internal metric; noisy alerts train responders to ignore them
- Reuse the project's existing observability stack, metric naming, and units — don't introduce a second system
- Never put **secrets or PII** into metric labels, span attributes, or alert payloads
- Keep instrumentation **low-overhead**; sample high-volume traces rather than recording every span
- Don't double-count or rename existing metrics in ways that break current dashboards and alerts
