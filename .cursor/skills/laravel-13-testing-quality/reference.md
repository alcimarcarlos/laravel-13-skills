# laravel-13-testing-quality Reference

## review quality

## Review Priorities

1. Behavior regressions.
2. Security/authorization/data leaks.
3. Data corruption or transaction gaps.
4. Performance issues such as N+1 queries or unbounded loads.
5. Missing tests for risky behavior.
6. Maintainability issues that create real confusion.

## Commands

Use what the project provides:

```bash
php artisan test
php artisan test --filter=Name
vendor/bin/pest
vendor/bin/phpunit
vendor/bin/pint
vendor/bin/phpstan analyse
composer test
composer lint
composer analyse
```

## Release Readiness

Before saying a feature is ready:

- Tests pass or known failures are explained.
- Formatting is clean.
- Static analysis is clean or existing baseline is respected.
- Migrations are reversible or intentionally documented.
- API behavior has status/error/authorization coverage.

## test patterns

## HTTP Feature Tests

Cover:

- Authenticated success path.
- Guest/unauthenticated behavior.
- Forbidden behavior for wrong user/role.
- Validation errors.
- Not found or ownership boundaries.
- Side effects: database writes, jobs, events, notifications.

## Factories

- Use factory states for business states.
- Avoid creating more data than the assertion needs.
- Prefer named users/roles over magic IDs.

## Fakes

Use:

- `Queue::fake()` or `Bus::fake()` for dispatched jobs.
- `Event::fake()` for domain events.
- `Notification::fake()` and `Mail::fake()` for outbound messages.
- `Storage::fake()` for upload/file flows.
- `Http::fake()` for external APIs.

Do not fake the thing being tested if the purpose is to verify integration with that subsystem.
