---
name: laravel-13-upgrade-rector
description: Use when upgrading a Laravel application or package to Laravel 13, migrating Laravel versions, configuring Rector with driftingly/rector-laravel, checking PHP 8.3+ requirements, resolving composer conflicts, planning dependency upgrades, or producing a Laravel 13 upgrade checklist.
---

# Laravel 13 Upgrade With Rector

## Workflow

1. Inspect `composer.json`, `composer.lock`, PHP constraint, Laravel constraint, `rector.php`, test config, static analysis config, `bootstrap/app.php`, and legacy kernel files.
2. Determine current Laravel version, target Laravel version, app vs package, PHP version, and blocking ecosystem packages.
3. For multi-major upgrades, prefer one major version at a time unless the user accepts a larger diff.
4. Install/configure Rector and `driftingly/rector-laravel`.
5. Run dry-run first, review diff, apply, clear caches, format, analyze, test.
6. Produce manual checklist for breaking changes Rector cannot handle.

## Laravel 13 Requirements

- PHP 8.3+.
- Composer 2.
- Align test stack with Pest 4 or PHPUnit 12 where applicable.
- Check Laravel ecosystem packages such as Sanctum, Passport, Cashier, Horizon, Telescope, Livewire, Filament, Larastan/PHPStan, Collision, and Tinker.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
