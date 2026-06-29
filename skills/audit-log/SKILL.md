---
name: audit-log
description: Record who did what, when, and to what — append-only, tamper-evident audit trails for security-sensitive actions, without logging secrets
---

# Audit Log

Record a trustworthy trail of security- and compliance-relevant actions (logins, permission changes, data access, deletions, money movement) so you can answer "who did what, when, and to what" after the fact, using the project's existing storage.

## What to capture

1. **The five Ws per event** — *who* (authenticated actor id, not just a display name), *what* (action/verb), *when* (server timestamp, UTC), *which* (target resource + id), and *context* (source IP, request/correlation id, before/after where relevant)
2. **Derive the actor from the verified session**, never from a client-supplied field — the actor identity is the whole point of an audit log; pair with the `auth` skill
3. **Log the outcome** — record denied/failed attempts too (failed logins, authorization denials), not just successes; those are often the most important signals
4. **Record meaningful actions, not noise** — audit sensitive state changes and access, not every read; keep it distinct from debug logging

## Integrity & access

1. **Append-only** — audit entries are never updated or deleted by application code; no in-place edits. Use an append-only table/store and restrict who can write
2. **Make it tamper-evident** for high-assurance needs — hash-chain entries or ship to write-once/external storage (e.g. a separate account) so tampering is detectable
3. **Restrict read access** — audit logs contain sensitive activity; gate viewing behind authorization and audit the auditors where required
4. **Set retention per policy** — keep entries for the required compliance window, then expire deliberately; document the period

## Pitfalls

1. **Never write secrets or sensitive payloads** into the audit trail — no passwords, tokens, full card/PII; log identifiers and changed-field names, not raw values. Pair with `secrets-scan`
2. **Don't lose events on failure** — decide whether an action proceeds if its audit write fails; for high-stakes actions, write the audit entry in the same transaction as the change so they commit together
3. **Keep audit logging separate from application logging** — different destination, retention, and access controls; pair with `logging` and `observability`

## Rules

- **Capture who/what/when/which/context** for every audited action, with the actor taken from the **verified session**, not client input
- **Make the store append-only** — no updates or deletes from app code; restrict write access
- **Log failures and denials**, not only successes — they're key security signals
- **Never record secrets or raw sensitive data** — log ids and field names, not values; pair with `secrets-scan`
- **Make it tamper-evident** (hash chain or write-once external store) when integrity matters
- **Write the audit entry atomically with the change** for high-stakes actions so they can't desync
- **Restrict who can read** the audit log and **set explicit retention** per compliance policy
- **Keep it separate from debug/application logs**; pair with `logging` and `observability`
- **Use a stable, queryable schema** (consistent action names, structured fields) so the trail is actually searchable after an incident; pair with `incident-response`
