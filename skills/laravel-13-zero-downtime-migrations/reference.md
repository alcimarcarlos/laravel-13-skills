# laravel-13-zero-downtime-migrations Reference

## Safe Column Addition (PostgreSQL & MySQL)

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        // Add nullable column - Instant in MySQL 8+ and Postgres
        Schema::table('orders', function (Blueprint $table) {
            $table->string('tracking_code')->nullable()->after('status');
        });
    }

    public function down(): void
    {
        Schema::table('orders', function (Blueprint $table) {
            $table->dropColumn('tracking_code');
        });
    }
};
```

## PostgreSQL Concurrent Index Creation

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Support\Facades\DB;

return new class extends Migration
{
    // PostgreSQL CONCURRENTLY cannot run inside a transaction block
    public $withinTransaction = false;

    public function up(): void
    {
        DB::statement('CREATE INDEX CONCURRENTLY IF NOT EXISTS idx_orders_user_created ON orders (user_id, created_at)');
    }

    public function down(): void
    {
        DB::statement('DROP INDEX CONCURRENTLY IF EXISTS idx_orders_user_created');
    }
};
```

## Batch Backfill Command Skeleton

```php
namespace App\Console\Commands;

use Illuminate\Console\Command;
use App\Models\Order;

class BackfillOrderTracking extends Command
{
    protected $signature = 'orders:backfill-tracking {--chunk=500} {--sleep=50000}';

    public function handle(): int
    {
        $chunk = (int) $this->option('chunk');
        $sleep = (int) $this->option('sleep'); // microseconds

        Order::whereNull('tracking_code')
            ->select(['id', 'order_number'])
            ->chunkById($chunk, function ($orders) use ($sleep) {
                foreach ($orders as $order) {
                    $order->updateQuietly(['tracking_code' => 'TRK-' . $order->order_number]);
                }
                usleep($sleep); // Allow DB to breathe between chunks
            });

        return Command::SUCCESS;
    }
}
```
