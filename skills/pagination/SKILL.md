---
name: pagination
description: Page through large result sets safely — choose cursor vs. offset, keep ordering stable, cap page size, and return consistent next/prev metadata
---

# Pagination

Return large collections in bounded pages so responses stay fast and memory-safe, using the project's existing API and query layer. Pick the strategy that fits the access pattern, and keep results stable as data changes.

## Choosing a strategy

1. **Prefer cursor (keyset) pagination for large or live data** — page on a stable sort key (e.g. `WHERE (created_at, id) < (:cursor)` `ORDER BY created_at, id`). It stays correct as rows are inserted/deleted and performs the same at page 1 and page 10,000
2. **Use offset/limit only for small, bounded, mostly-static sets** — `OFFSET` scans and discards all skipped rows, so deep pages get slow, and inserts/deletes shift rows between pages (items skipped or shown twice)
3. **Never load everything and slice in memory** — page in the query, not after fetching the full table

## Implementing it well

1. **Always order by a unique, stable key** — append a tiebreaker like `id` so rows with equal sort values have a deterministic order; without it, pages overlap or skip
2. **Cap and default the page size** — enforce a max (e.g. 100) and a sensible default; reject or clamp oversized requests so a client can't ask for everything
3. **Make cursors opaque and validated** — encode the key (e.g. base64) so clients treat it as a token; validate/decode safely and reject tampered cursors
4. **Return clear paging metadata** — include `next`/`prev` cursors (or a `has_more` flag); avoid an expensive total `COUNT` on huge tables unless the client truly needs it
5. **Index the sort key** — the `ORDER BY`/cursor columns must be indexed or pagination degrades to full scans; pair with the `optimize` skill

## Rules

- **Default to cursor/keyset pagination** for large or frequently-changing data; reserve offset for small static sets
- **Order by a unique, stable key** (add `id` as a tiebreaker) so pages don't overlap, skip, or reorder
- **Always cap page size** with a max and a default — never let a client request an unbounded result
- **Page in the query**, never by fetching all rows and slicing in memory
- **Make cursors opaque and validate them** — treat them as tokens, reject malformed/tampered values
- **Index the columns you sort/cursor on**; pair with `optimize`
- **Avoid `COUNT(*)` totals on large tables** unless required — use `has_more` instead
- **Keep paging semantics consistent** across endpoints (same param names, same metadata shape); pair with `api-design`
- **Test boundaries** — empty results, last page, duplicate sort values, and concurrent inserts; pair with `test-gen`
