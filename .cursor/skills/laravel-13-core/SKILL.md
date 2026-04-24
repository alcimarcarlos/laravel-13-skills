---
name: laravel-13-core
description: Use for Laravel 13 backend development in PHP 8.3+, including controllers, models, routes, Form Requests, services/actions, jobs, events, policies, migrations, factories, Blade-adjacent backend code, code review, and architecture decisions. Trigger whenever building, modifying, or reviewing ordinary Laravel application code.
---

# Laravel 13 Core

## Workflow

1. Inspect `composer.json`, Laravel version constraints, PHP constraint, and neighboring files.
2. Identify the slice: HTTP, model/domain, persistence, async, event, console, or integration.
3. Follow the application's existing conventions first. Apply these defaults only where no local pattern is clear.
4. Design from the framework boundary inward: route/request -> policy -> controller/action/service -> model/query -> resource/response -> test.
5. Validate with the smallest useful command set from `reference.md`.

## Defaults

- Target Laravel 13 and PHP 8.3+.
- Add `declare(strict_types=1);` to new PHP files when the project uses it.
- Type parameters and return values.
- Keep controllers thin: validate/authorize, call an action/service when behavior is non-trivial, return a response/resource/redirect.
- Use Form Requests for validation and request authorization.
- Use Policies for authorization decisions.
- Keep Eloquent models focused on relationships, casts, scopes, accessors/mutators, factories, and domain-safe helpers.
- Prefer Laravel conventions over custom abstraction. Add repositories only for external persistence boundaries or when the project already uses them.
- Use transactions for multi-write invariants.
- Use queues for slow work after the response unless the user flow requires synchronous execution.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
