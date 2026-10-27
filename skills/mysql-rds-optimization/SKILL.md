---
name: mysql-rds-optimization
description: "MySQL 8+ and AWS RDS/Aurora optimization: connection pooling, RDS Proxy, gap lock/deadlock prevention, InnoDB transaction isolation, EXPLAIN ANALYZE, replica lag mitigation, and IOPS tuning."
license: UNLICENSED
---

# MySQL & AWS RDS / Aurora Optimization

## Objective

Prevent database connection exhaustion, query deadlocks, and latency spikes in production applications using MySQL 8+ and AWS RDS or Aurora.

## Compatibility (Cursor, Codex, Claude Code)

- Tool-agnostic architecture and query optimization playbook.
- Target: MySQL 8.0+, AWS RDS for MySQL, and Amazon Aurora MySQL.
- Always verify execution plans with `EXPLAIN` or `EXPLAIN ANALYZE` before concluding an optimization.

## Golden Rules

### 1. Connection Management & RDS Proxy
- **Prevent Connection Starvation**: Web workers and async queues must not open unbounded connections.
- **RDS Proxy**: Use AWS RDS Proxy when deploying to serverless (Lambda), container clusters (ECS/Fargate), or high-concurrency runtimes (Laravel Octane / FrankenPHP).
- **Avoid Connection Pinning**: Session-level variables (`SET @var`), temporary tables, and prepared statements without proper resets pin connections to specific RDS instances, negating proxy multiplexing.

### 2. Transaction Isolation & Deadlock Prevention
- **InnoDB Locking**: MySQL uses Next-Key Locking (record lock + gap lock) under default `REPEATABLE READ`. Missing indexes on `WHERE` clauses turn row locks into whole-table gap locks.
- **Lock Ordering**: Always acquire locks on multiple records in a deterministic order (e.g., sort record IDs in ascending order: `ORDER BY id ASC FOR UPDATE`) to eliminate circular deadlock cycles.
- **Concurrent Queue/Worker Consumption**: Always use `SELECT ... FOR UPDATE SKIP LOCKED` for task processing so concurrent workers never block each other.
- **Transaction Duration**: Never perform external HTTP calls, slow computations, or disk I/O while holding open transactions or row locks.

### 3. Read-Replicas & Replica Lag (Read-After-Write)
- **Asynchronous Replication Lag**: Replica data can lag hundreds of milliseconds behind the primary (writer).
- **Sticky Connections**: When a user performs a write (`POST`/`PUT`/`DELETE`), ensure subsequent reads within the same request or immediate redirect use the primary writer connection (e.g., Laravel's `'sticky' => true`).

### 4. Index Design & Execution Plans
- **The ESR Rule**: For multi-column composite indexes, order columns: **E**quality (`=`) -> **S**ort (`ORDER BY`) -> **R**ange (`>`, `<`, `BETWEEN`).
- **Covering Indexes**: Design indexes where all selected columns exist inside the index tree (`Using index`), avoiding clustered index pointer traversals.
- **EXPLAIN Verification**: Never accept a query with `type: ALL` (full table scan) or `Using filesort` on large tables.

## Additional Resources

- Read `reference.md` for connection configs, deadlock debugging, and EXPLAIN analysis.
