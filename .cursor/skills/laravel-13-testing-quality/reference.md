# laravel-13-testing-quality Reference

## PHP-FIG PSR Touchpoints

- Use Pint as the practical PSR-12 gate when the project has it configured.
- When code depends on `Psr\Log\LoggerInterface`, assert sensitive values are not logged and inject a fake/test logger where useful.
- When code depends on `Psr\Http\Client\ClientInterface`, test request shape, error mapping, timeout/retry behavior, and response handling through a fake adapter.
- When code depends on `Psr\Clock\ClockInterface`, inject a fixed clock instead of relying on wall-clock time.
- Static analysis should prefer native PHP types first, with PHPDoc added only where generics, array shapes, or framework magic need it.

## Review + Quality

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

## Test Patterns

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

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
