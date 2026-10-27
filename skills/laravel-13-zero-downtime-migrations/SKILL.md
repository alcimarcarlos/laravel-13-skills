---
name: laravel-13-zero-downtime-migrations
description: "Zero-downtime migrations in Laravel: Expand/Contract, concurrent indexes, backfills. Trigger on DB schema changes."
license: UNLICENSED
---

# Laravel 13 Zero-Downtime Migrations

## Objective

Prevent database lockups, connection starvation, and downtime when running schema migrations and data transformations in production Laravel applications.

## Compatibility (Cursor, Codex, Claude Code)

- Tool-agnostic playbook: applies to MySQL 8+ and PostgreSQL 14+.
- Never execute large, blocking table alters in a single transaction.
- When adding columns, foreign keys, or indexes to large tables, strictly follow the Expand/Contract phased approach.

## Golden Rules

1. **Never Add Non-Nullable Columns Without Defaults**:
   - Adding a `NOT NULL` column without a default requires a full table rewrite and exclusive lock.
   - Padrão: Adicione a coluna como `nullable()`, preencha os dados em lotes (backfill em jobs), adicione a constraint posteriormente ou defina defaults que não exijam rewrite.
2. **Never Run Massive Backfills in Migrations**:
   - Never run `UPDATE orders SET ...` directly inside an `up()` migration method.
   - Run backfills through chunked console commands or Oban/Queue workers with rate limits and transaction isolation.
3. **Non-Blocking Index Creation**:
   - In PostgreSQL, creating an index locks the table against writes. Use raw SQL with `CONCURRENTLY` outside transactions when indexing large tables.
4. **Phased Column / Table Deletions (Expand & Contract)**:
   - **Phase 1 (Expand)**: Add the new column/table, deploy code that writes to both old and new.
   - **Phase 2 (Backfill)**: Backfill historical rows in background batches.
   - **Phase 3 (Switch)**: Deploy code that reads and writes exclusively to the new column. Ignore the old column in Eloquent via `$hidden` or schema overrides.
   - **Phase 4 (Contract)**: Drop the old column in a final, isolated migration.

## Safe Migration Checklist

- [ ] Does this table have > 100,000 rows? If yes, treat as high-risk.
- [ ] Are new columns `nullable()` or non-blocking?
- [ ] Is foreign key creation isolated from data updates?
- [ ] Are indexes created without blocking concurrent writes?
- [ ] Is the migration reversible in `down()`?

## Additional Resources

- Read `reference.md` for code examples and phased deployment templates.
