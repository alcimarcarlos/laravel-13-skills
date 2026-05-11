# Laravel 13 Guidelines

## Reference Stack

- Laravel 13
- PHP 8.3+
- Composer 2
- Pest 4 or PHPUnit 12
- Laravel Pint
- Larastan/PHPStan when configured
- Laravel Boost when available

## General Conventions

- Local consistency is more important than a generic preference.
- Apply accepted PHP-FIG PSRs where they improve interoperability, especially PSR-1, PSR-3, PSR-4, PSR-12, and package-boundary contracts. See `docs/php-fig-psr-map.md`.
- Controllers receive the request, authorize/validate through Form Requests, delegate behavior, and return a response.
- Actions/services are useful when they reduce real complexity or reuse behavior outside HTTP.
- Models hold relationships, casts, scopes, and safe helpers.
- Repositories should be used only when the project already uses them or when there is a real external persistence boundary.
- Events/listeners are useful for side effects.
- Jobs should be idempotent when they can be retried.

## PHP-FIG PSR Defaults

- Prefer accepted PSRs over draft, deprecated, or abandoned recommendations.
- Use PSR-4 autoloading through Composer; avoid manual class includes.
- Use Laravel Pint for PSR-12-compatible formatting when configured.
- Type against PSR interfaces only at useful boundaries: logging, cache, container, HTTP clients/messages, event dispatchers, and clocks.
- Keep ordinary Laravel application code Laravel-native unless an installed package or explicit integration contract requires a PSR interface.

## Validation Gates

Use commands that exist in the target project:

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

## Sources Used During Curation

This package was consolidated and deduplicated from public Laravel skill sources:

- `AsyrafHussin/agent-skills`
- `laravel/agent-skills`
- `laravel/boost`
- `JustSteveKing/api-skill`
- `masterfermin02/laravel-agent-skill`
- `ikamal7/agent-skills`
- `jpcaparas/superpowers-laravel`

The final result is not a mirror of those sources. The instructions were regrouped by development context to keep the structure compatible with the pattern used in the `ionic-capacitor-skills` repository.
