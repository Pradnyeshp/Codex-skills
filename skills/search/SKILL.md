---
name: search
description: Add full-text or filtered search — pick the right engine, index the right fields, rank by relevance, and paginate results without leaking unauthorized data
---

# Search

Add search over a collection (products, docs, users) so users find what they need quickly and relevantly, using the project's existing datastore or search engine (e.g. Postgres full-text, Elasticsearch/OpenSearch, Meilisearch, Typesense) rather than naive `LIKE '%term%'` scans.

## Choosing the approach

1. **Match the tool to the need** — exact/prefix filters and small datasets fit the primary DB with proper indexes; relevance-ranked full-text over large corpora fits a dedicated search engine. Don't reach for a cluster you don't need, or `LIKE` for real search
2. **Avoid leading-wildcard `LIKE` scans** — `LIKE '%term%'` can't use a normal index and gets slower with data; use full-text indexes (e.g. Postgres `tsvector` + GIN) or an engine
3. **Model the index deliberately** — decide which fields are searchable, which are filterable (facets), and which are just returned; index accordingly instead of dumping every column

## Quality & relevance

1. **Analyze text consistently** — apply the same tokenization, lowercasing, stemming, and stop-word handling at index and query time so matches line up
2. **Rank by relevance, then tie-break deterministically** — score by term frequency/field weights (title > body), and add a stable tiebreaker (e.g. `id`) so ordering is repeatable; pair with the `pagination` skill
3. **Handle typos and partial input** where it matters — prefix/fuzzy matching for as-you-type, but keep it bounded so it stays fast
4. **Return highlights and useful metadata** so users see *why* a result matched

## Correctness & safety

1. **Filter by authorization in the query** — apply the user's access scope as a filter so search never returns documents they can't see; never rely on hiding them in the UI. Pair with the `auth` skill
2. **Keep the index fresh** — update/delete index entries when source data changes (sync on write or via a background job); stale search results erode trust. Pair with `background-jobs`
3. **Paginate and cap result size** — never return unbounded hits; pair with `pagination`
4. **Validate and bound query input** — cap query length and sanitize; don't build engine queries by string-concatenating user input (injection). Pair with `data-validation`

## Rules

- **Don't implement real search with `LIKE '%…%'`** — use full-text indexes or a search engine so it stays fast and relevant
- **Use the same analyzer at index and query time** so tokenization/stemming match
- **Apply the user's authorization scope as a query filter** — search must never surface unauthorized documents; pair with `auth`
- **Rank by relevance with a deterministic tiebreaker**, and **paginate** results; pair with `pagination`
- **Keep the index in sync** with source data on writes/deletes, ideally via a background job; pair with `background-jobs`
- **Validate, bound, and never string-concat** user query input into engine queries; pair with `data-validation`
- **Index only what you need** (searchable vs. filterable vs. stored) to keep the index lean and fast
- Make search **observable** — track latency, zero-result rate, and popular queries to improve relevance; pair with `observability`
- **Test relevance and edge cases** — empty query, no results, special characters, and access filtering; pair with `test-gen`
