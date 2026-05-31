# laravel-13-data-performance Reference

## PHP-FIG PSR Touchpoints

- PSR-6 cache pools are useful for package-style components that need cache item metadata, deferred saves, or pool interoperability.
- PSR-16 simple cache is useful for small portable services that only need key/value cache operations.
- Prefer Laravel cache stores, tags, locks, and helpers for application code unless an explicit interface boundary calls for PSR-6 or PSR-16.
- Use `Psr\Clock\ClockInterface` when cache expiry, retry windows, or idempotency windows need injectable portable time.

## Cache + Queue + Consistency

## Cache

- Cache expensive reads, not authorization decisions with user-specific risk.
- Include tenant/user/locale/version in cache keys when relevant.
- Use tags only when the configured cache driver supports them.
- Prefer explicit invalidation for long TTLs; use short TTLs when invalidation is uncertain.

## Cache Stampede Protection

- Wrap expensive recomputation in an atomic lock so only one worker rebuilds a hot key while others wait or serve stale data.
- Prefer single-flight `Cache::remember` plus a lock for fan-out reads; avoid many workers recomputing the same value on expiry.
- Stagger TTLs (add jitter) for keys that would otherwise expire simultaneously.
- Use `Cache::lock(...)->block()` with a short timeout for critical sections; release in a `finally` path so locks never leak.

## Queues

- Jobs should be idempotent.
- Use unique jobs or locks for duplicate-prone workflows.
- Set tries, backoff, timeout, and queue names intentionally.
- Keep serialized payloads small. Pass model IDs rather than full object graphs.

## Transactions

- Wrap related writes that must succeed or fail together.
- Dispatch jobs after commit when they depend on committed data.
- Avoid external HTTP calls inside database transactions.

## Queries

## N+1 Prevention

- Use `with()` for relationships accessed after the query.
- Use constrained eager loading for filtered relationships.
- Use `loadMissing()` only when objects already exist and eager loading at source is not practical.
- Never query from Blade loops or resource loops without checking loaded relationships.

## Aggregates

- Use `withCount()` for counts.
- Use aggregate subqueries for latest/first related values.
- Prefer one clear indexed query over clever query chains that hide expensive work.

## Large Reads

- Use `chunkById()` or `lazyById()` when mutating rows while iterating.
- Avoid plain `chunk()` while updating the same rows you filter on: shifting offsets skip records. Use `chunkById()` instead.
- Use `cursor()` for read-only streaming when ordering is stable.
- Avoid loading full relation graphs for exports.

## Bulk Writes

- Replace per-row `create`/`save` loops with `insert()` or `upsert()` for large volumes; mind that bulk inserts bypass model events and timestamps.
- Batch updates by stable key ranges rather than one query per row.
- For mixed insert/update, define the unique-by columns and the columns to update in `upsert()`.

## Read/Write Splitting

- When read replicas are configured, route heavy read traffic to replicas and writes to the primary.
- Account for replica lag: read-after-write paths that must be consistent should hit the primary or use `sticky` connections.

## Timeouts and Resilience

- Set bounded database connection/statement timeouts so a slow query cannot exhaust the worker pool.
- Set explicit timeouts (and retries with backoff) on outbound HTTP clients; treat upstreams as unreliable.
- Retry deadlocks and lock-wait timeouts with capped, jittered backoff; ensure the unit of work is idempotent before retrying.

## Long-Lived Workers (Octane/queues)

- Do not store per-request/user state in singletons, static properties, or container-shared instances.
- Reset or re-resolve stateful services between requests/jobs; watch for growing in-memory caches and unbounded collections.

## Debugging

- Inspect generated SQL and bindings.
- Check query count in tests or debug tools.
- Use database `EXPLAIN` for slow queries.

## Schema + Indexes

## Index Selection

Add indexes for:

- Foreign keys used in joins and lookups.
- Columns used in frequent `where` filters.
- Compound filter/order combinations.
- Unique business identifiers.
- Cursor pagination keys.

Avoid indexes that:

- Duplicate another prefix index without benefit.
- Slow heavy writes without a matching read path.
- Encode speculative queries no screen/job actually runs.

## Migration Safety

- Keep migrations reversible where possible.
- For large tables, split add-column, backfill, constraint, and not-null changes.
- Avoid destructive changes without data migration and rollback plan.
- Prefer nullable columns with safe defaults during rolling deploys.

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
