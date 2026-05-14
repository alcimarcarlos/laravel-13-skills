---
name: laravel-13-data-performance
description: "Use for Laravel 13 database, Eloquent, caching, performance, queues, Horizon, transactions, indexing, N+1 fixes, chunking, cursor pagination, Redis/cache design, query optimization, large datasets, and zero-downtime migration concerns."
license: UNLICENSED
---

# Laravel 13 Data Performance

## Workflow

1. Identify the hot path: request, job, command, report, export, or dashboard.
2. Inspect current queries, relationships, indexes, cache use, queue behavior, and tests.
3. Fix correctness first, then query count, memory, index use, cache safety, and async behavior.
4. Prefer measurable changes: query count, explain plans, response time, memory, queue time, or lock duration.
5. Add regression tests for N+1, filtering, pagination, transactions, or job behavior when feasible.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Prefer **measurable** improvements (query count, EXPLAIN, memory) over speculative refactors.
- Keep performance guidance **environment-safe**: no assumptions about local tooling beyond standard Laravel/PHP commands.

## Defaults

- Eager load relationships used in loops.
- Use `withCount`, `withSum`, or subqueries for aggregate display.
- Select only needed columns in high-volume reads.
- Use `chunkById`, `lazyById`, or cursors for large processing.
- Add compound indexes that match real filters and ordering.
- Cache computed reads with explicit invalidation or short TTLs.
- Use transactions for multi-write invariants.
- Queue slow, retryable work and make jobs idempotent.
- Use PSR-6/16 cache interfaces only for interoperable package boundaries; prefer Laravel cache APIs for ordinary application work.

## Safety Defaults (correctness + security)

- Do not cache **authorization decisions** unless the cache key fully captures user/tenant/ability scope.
- Do not move external HTTP calls inside transactions; dispatch after commit when needed.
- When adding indexes/migrations on large tables, prefer **low-lock** patterns and reversible steps.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
