# laravel-13-systematic-debugging Reference

## Diagnostic Commands

```bash
# Check recent error logs
tail -n 100 storage/logs/laravel.log

# Run targeted test with full exception trace
php artisan test --filter=OrderTest --stop-on-failure

# Clear and re-cache configs and routes during debugging
php artisan config:clear
php artisan route:clear
```

## Minimal Failing Test Template (Pest)

```php
test('reproduces issue #123: discount code calculation with zero quantity', function () {
    $user = User::factory()->create();

    // Trigger exact edge condition that failed
    $response = $this->actingAs($user)->postJson('/api/v1/orders/calculate', [
        'items' => [],
        'coupon_code' => 'DISCOUNT50',
    ]);

    // Assert expected correct behavior (should fail before fix is applied)
    $response->assertStatus(422)
        ->assertJsonValidationErrors(['items']);
});
```
