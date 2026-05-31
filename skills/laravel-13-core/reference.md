# laravel-13-core Reference

## Architecture

## PHP-FIG PSR Baseline

- Apply PSR-1 and PSR-12-compatible style for PHP files, using Laravel Pint or the project's configured formatter.
- Keep Composer PSR-4 autoload mappings aligned with namespaces and paths.
- Prefer constructor injection over service-location. Use `Psr\Container\ContainerInterface` only for framework-agnostic code that genuinely needs a container contract.
- Use Laravel events/listeners for application code. Use PSR-14 only when building package-style event integrations.
- Use `Psr\Clock\ClockInterface` when a service needs portable injectable time; otherwise follow the project's Laravel time-testing pattern.
- Do not revive PSR-0 or PSR-2 conventions in new code.

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

## Reliability and Failure Handling

- Group writes that must succeed together in a transaction; commit before dispatching dependent jobs/events.
- Keep transactions short: compute and validate first, hold locks for the minimum time, never await external services inside them.
- Treat retries as at-least-once: guard side effects with idempotency keys, unique constraints, or status checks.
- For queued work, set `tries`, `backoff`, and `timeout`, and implement `failed()` for cleanup/alerting; let unrecoverable jobs fail loudly rather than catch-and-ignore.
- Convert transient failures (deadlocks, lock waits) into bounded, jittered retries; surface persistent failures.
- Return safe error responses (no internals/stack traces/secrets); rely on the framework exception handler for structured logging.

## Eloquent + Schema

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

## Quality Gates

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

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
