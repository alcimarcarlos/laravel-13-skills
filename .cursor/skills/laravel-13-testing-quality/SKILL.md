---
name: laravel-13-testing-quality
description: Use for Laravel 13 test writing, Pest 4, PHPUnit 12, factories, feature/unit tests, HTTP tests, database assertions, facade fakes, code review, Pint, Larastan/PHPStan, quality gates, CI readiness, and release validation.
---

# Laravel 13 Testing and Quality

## Workflow

1. Detect Pest or PHPUnit from `composer.json`, `tests/`, and config files.
2. Follow existing test style.
3. Cover the behavior that can regress, not every line.
4. Use factories, fakes, and database assertions instead of brittle implementation assertions.
5. Run targeted tests first, then formatting/static analysis if available.

## Defaults

- Prefer feature tests for HTTP behavior and integration boundaries.
- Use unit tests for pure domain logic, value objects, and isolated services.
- Use factories for model setup.
- Use Laravel fakes for Mail, Notification, Queue, Event, Bus, Storage, HTTP, and AI SDK where applicable.
- Assert status, authorization, validation errors, response JSON/resource shape, database state, and dispatched side effects.
- For code review, lead with behavioral bugs, security risks, and missing tests.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
