# laravel-13-upgrade-rector Reference

## checklist

Before changing code:

- Confirm PHP is 8.3+.
- Confirm Composer is 2.x.
- Identify current Laravel version from lockfile, not only constraints.
- Check package blockers with `composer why-not laravel/framework ^13.0`.
- Confirm test command.
- Confirm CI PHP version.

During upgrade:

- Update PHP and Laravel constraints.
- Align Pest/PHPUnit and Laravel ecosystem packages.
- Run Rector dry-run.
- Review generated changes.
- Apply in reviewable commits if using Git.

After upgrade:

- Clear config/routes/cache.
- Run route list and app config sanity checks.
- Run Pint.
- Run static analysis if configured.
- Run targeted and full tests where feasible.
- Review official Laravel 13 upgrade guide for manual framework changes.

## rector

Install:

```bash
composer require rector/rector --dev
composer require driftingly/rector-laravel --dev
```

Check blockers:

```bash
composer why-not laravel/framework ^13.0
```

Example `rector.php` for an app:

```php
<?php

declare(strict_types=1);

use Rector\Config\RectorConfig;
use RectorLaravel\Set\LaravelSetList;

return RectorConfig::configure()
    ->withPaths([
        __DIR__ . '/app',
        __DIR__ . '/database',
        __DIR__ . '/routes',
        __DIR__ . '/config',
        __DIR__ . '/tests',
    ])
    ->withSkip([
        __DIR__ . '/vendor',
        __DIR__ . '/storage',
        __DIR__ . '/bootstrap/cache',
        __DIR__ . '/public',
    ])
    ->withSets([
        LaravelSetList::LARAVEL_130,
    ]);
```

Execution:

```bash
vendor/bin/rector process --dry-run
vendor/bin/rector process
php artisan config:clear
php artisan route:clear
php artisan cache:clear
vendor/bin/pint
php artisan test
```

For packages, use `src` and `tests` paths instead of app-only directories.
