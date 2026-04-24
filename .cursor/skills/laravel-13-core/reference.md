# laravel-13-core Reference

## architecture

## HTTP Layer

- Routes should be explicit, named, and grouped by middleware/domain.
- Controllers should stay small and readable.
- Use invokable controllers for single-purpose endpoints when the codebase does that already.
- Use resource controllers when the project has RESTful resource convention and the methods remain thin.
- Put validation and authorization in Form Requests for write operations.
- Return API Resources for structured API output.

## Application Layer

Use an action/service when:

- More than one model or external system participates.
- A transaction is needed.
- The same behavior is called from HTTP, jobs, console, or listeners.
- The controller would otherwise contain branching business rules.

Avoid an action/service when:

- The controller only delegates one trivial Eloquent call.
- The abstraction name repeats the method name without reducing complexity.

## Domain Boundaries

- Prefer feature/domain folders only when the project already groups by feature or the module is large enough to justify it.
- Events/listeners are good for side effects after core state changes.
- Jobs should be idempotent where retries are possible.
- Avoid hidden global helpers for business behavior; inject dependencies or use framework facades where local style allows.

## eloquent schema

## Models

- Define relationships with return types such as `BelongsTo`, `HasMany`, `BelongsToMany`, and `MorphMany`.
- Use `$casts` for dates, booleans, enums, JSON, encrypted values, and value objects.
- Guard mass assignment with `$fillable` or local project convention.
- Add query scopes for reusable filtering logic.
- Prevent accidental N+1 queries in development with strict model settings where the project supports it.

## Migrations

- Choose column types intentionally.
- Add indexes for frequent `where`, `join`, `orderBy`, unique lookup, and foreign-key access patterns.
- Use foreign keys and cascade behavior when lifecycle ownership is clear.
- For existing large tables, prefer safe, reversible, low-lock changes.

## Factories and Seeders

- Use factories in tests rather than hand-building model arrays.
- Keep factory states named after real business states.
- Seed only deterministic reference data unless the task explicitly needs fake volume.

## Consistency

Before changing a pattern, inspect similar files:

- Existing model casts and fillable style.
- Migration naming and index naming.
- Factory state style.
- Whether the project uses service classes, action classes, or direct model orchestration.

## quality gates

Pick commands that exist in the target project:

```bash
php artisan test
php artisan test --filter=SpecificTest
vendor/bin/pest
vendor/bin/phpunit
vendor/bin/pint
vendor/bin/phpstan analyse
composer test
composer lint
composer analyse
php artisan route:list
php artisan config:show app
```

For risky changes:

- Run targeted tests first.
- Run formatting.
- Run static analysis if configured.
- Run the broader suite only when the touched surface is shared or high risk.
