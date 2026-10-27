# mysql-rds-optimization Reference

## Laravel Database Configuration for RDS Read/Write Splitting & Sticky Reads

```php
// config/database.php
'mysql' => [
    'driver' => 'mysql',
    'read' => [
        'host' => [env('DB_READ_HOST', 'aurora-reader.cluster-ro-xyz.rds.amazonaws.com')],
    ],
    'write' => [
        'host' => [env('DB_HOST', 'aurora-writer.cluster-xyz.rds.amazonaws.com')],
    ],
    'sticky' => true, // Prevents reading stale data immediately after a write
    'database' => env('DB_DATABASE', 'forge'),
    'username' => env('DB_USERNAME', 'forge'),
    'password' => env('DB_PASSWORD', ''),
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '',
    'strict' => true,
    'engine' => null,
    'options' => extension_loaded('pdo_mysql') ? array_filter([
        PDO::ATTR_EMULATE_PREPARES => true,
        PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
    ]) : [],
],
```

## Safe Worker Consumption with SKIP LOCKED

```php
// Locks only unprocessed orders without blocking other concurrent workers
$order = DB::transaction(function () {
    return Order::where('status', 'pending')
        ->orderBy('id', 'asc')
        ->lock('FOR UPDATE SKIP LOCKED')
        ->first();
});
```

## Analyzing Execution Plans in MySQL 8+

```sql
-- Measure exact execution cost, memory, and engine loop count
EXPLAIN ANALYZE
SELECT o.id, o.status, o.total_amount
FROM orders o
WHERE o.user_id = 42 AND o.status = 'completed'
ORDER BY o.created_at DESC
LIMIT 20;

-- Optimal Composite Index for the query above (ESR Rule):
-- user_id (= Equality), status (= Equality), created_at (Sort)
CREATE INDEX idx_orders_user_status_created 
ON orders (user_id, status, created_at DESC);
```

## Investigating InnoDB Deadlocks

```sql
-- Shows the last detected deadlock, including lock types and conflicting queries
SHOW ENGINE INNODB STATUS;
```
