---
name: redis-architecture-caching
description: "Redis and AWS ElastiCache architecture: eviction safety (maxmemory-policy), keyspace hygiene (SCAN vs KEYS), cache stampede mitigation, distributed atomic locks, pipelines, and data structures."
license: UNLICENSED
---

# Redis & AWS ElastiCache Architecture

## Objective

Ensure production-grade stability, zero data loss, and sub-millisecond latency when using Redis or AWS ElastiCache for caching, queues, sessions, and atomic locks.

## Compatibility (Cursor, Codex, Claude Code)

- Tool-agnostic playbook: applies to standalone Redis 7+, AWS ElastiCache, and Redis Cluster.
- Never treat Redis as a crash-proof relational database; design for ephemeral cache invalidation and intentional queue persistence.

## Golden Rules

### 1. The Eviction Disaster Rule (maxmemory-policy)
- **CRITICAL**: Never share a single Redis instance between volatile Cache and durable Queues/Sessions when eviction is enabled (`allkeys-lru`, `volatile-lru`).
- If memory fills up, Redis will silently **evict queued jobs, auth tokens, and active sessions** to make room for cached queries!
- **Architecture Mandate**: Separate Redis into two dedicated instances/clusters:
  1. **Cache Instance**: Configured with `maxmemory-policy: allkeys-lru` or `volatile-lfu`.
  2. **Queues / Sessions / Locks Instance**: Configured with `maxmemory-policy: noeviction` (throws OOM error on writes instead of deleting queue jobs).

### 2. Strict Prohibition of `KEYS *`
- Redis is single-threaded. Running `KEYS pattern*` blocks the event loop for the duration of the scan. In production with > 50,000 keys, this freezes the application and causes timeouts.
- **Mandatory Pattern**: Always use cursor-based iteration with `SCAN` (or use tagged sets/hashes).

### 3. Cache Stampede Mitigation (Dog-Piling)
- When a hot cache key expires, dozens of concurrent requests simultaneously hit the database to rebuild the cache, saturating DB connections.
- **Mandatory Mitigation**:
  - Use **Atomic Locks** (`Cache::lock()` or Redis `SET NX PX`) so only one worker recalculates while others wait or return stale fallback data.
  - Or use **Probabilistic Early Expiration (XFetch)** to refresh data before it officially expires.

### 4. Distributed Atomic Locks
- Always acquire locks atomically:
  ```
  SET lock:resource_id random_token NX PX 30000
  ```
- **Safe Release**: Always release locks via a Lua script that verifies the token matches the current owner, preventing a slow process from unlocking a newer lock acquired by another process.

### 5. Network Round-Trip Reduction (Pipelines & MGET)
- Avoid issuing multiple individual `GET` or `SET` commands inside a loop over the network.
- Use `Redis::pipeline()` or `MGET`/`MSET` to bundle multiple commands into a single TCP round-trip.

## Additional Resources

- Read `reference.md` for lock release Lua scripts, SCAN helpers, and cache stampede wrappers.
