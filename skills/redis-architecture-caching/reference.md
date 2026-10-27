# redis-architecture-caching Reference

## Safe Pattern: Invalidation with SCAN (Never KEYS)

```php
// Invalidate cache keys matching a pattern without blocking the Redis server
function safeDeletePattern(string $pattern): int
{
    $deleted = 0;
    $cursor = '0';
    $redis = Illuminate\Support\Facades\Redis::connection('cache');

    do {
        [$cursor, $keys] = $redis->scan($cursor, ['match' => $pattern, 'count' => 500]);
        if (!empty($keys)) {
            $redis->del(...$keys);
            $deleted += count($keys);
        }
    } while ($cursor !== '0');

    return $deleted;
}
```

## Cache Stampede Prevention with Atomic Lock

```php
use Illuminate\Support\Facades\Cache;

function getPopularCatalogWithStampedeProtection(int $tenantId): array
{
    $cacheKey = "tenant:{$tenantId}:catalog:popular";

    return Cache::store('redis')->get($cacheKey, function () use ($cacheKey, $tenantId) {
        // Acquire an atomic lock for 10 seconds while calculating
        $lock = Cache::lock("lock:{$cacheKey}", 10);

        try {
            // Block for up to 3 seconds waiting for the lock
            $lock->block(3);

            // Double check if another worker refreshed the key while we were waiting
            if ($cached = Cache::store('redis')->get($cacheKey)) {
                return $cached;
            }

            // Perform expensive query
            $data = DB::table('products')->where('tenant_id', $tenantId)->get()->toArray();

            // Store for 1 hour
            Cache::store('redis')->put($cacheKey, $data, 3600);

            return $data;
        } finally {
            optional($lock)->release();
        }
    });
}
```

## Atomic Lock Release via Lua Script

```lua
-- Releases lock ONLY if the token matches the value set by this caller
if redis.call("get", KEYS[1]) == ARGV[1] then
    return redis.call("del", KEYS[1])
else
    return 0
end
```

## Pipeline Batching Example

```php
use Illuminate\Support\Facades\Redis;

// Bundles 100 SET commands into a single network packet
Redis::pipeline(function ($pipe) use ($userScores) {
    foreach ($userScores as $userId => $score) {
        $pipe->zAdd('leaderboard:daily', $score, $userId);
    }
});
```
