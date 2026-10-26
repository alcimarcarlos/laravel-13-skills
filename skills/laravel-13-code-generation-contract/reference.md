# laravel-13-code-generation-contract Reference

## Standard Slice Template Checklist

When implementing an API endpoint or domain feature, follow this order:

- [ ] **1. Route**: `routes/api.php` with proper middleware (`auth:sanctum`, rate-limiting).
- [ ] **2. Form Request**: `app/Http/Requests/...` with typed `rules()` and explicit `authorize()`.
- [ ] **3. Policy**: `app/Policies/...` attached to the model, verifying tenant and ownership.
- [ ] **4. Action / Service**: `app/Actions/...` encapsulating domain business logic.
- [ ] **5. Resource / Response**: `app/Http/Resources/...` transforming the model to JSON.
- [ ] **6. Controller**: `app/Http/Controllers/...` invoking the Action and returning Resource.
- [ ] **7. Pest Test**: `tests/Feature/...` testing 200, 401, 403, and 422.

## Minimal Pest Test Skeleton

```php
<?php

use App\Models\User;
use App\Models\Order;

test('guest cannot create order', function () {
    $this->postJson('/api/v1/orders', [])
        ->assertUnauthorized();
});

test('user cannot create order with invalid payload', function () {
    $user = User::factory()->create();

    $this->actingAs($user)
        ->postJson('/api/v1/orders', ['items' => []])
        ->assertUnprocessable()
        ->assertJsonValidationErrors(['items']);
});

test('user can create order successfully', function () {
    $user = User::factory()->create();

    $this->actingAs($user)
        ->postJson('/api/v1/orders', [
            'items' => [['id' => 1, 'quantity' => 2]],
        ])
        ->assertCreated()
        ->assertJsonPath('data.status', 'pending');
});
```
