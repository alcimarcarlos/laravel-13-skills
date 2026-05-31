---
name: laravel-13-core
description: "Use for Laravel 13 backend development in PHP 8.3+, including controllers, models, routes, Form Requests, services/actions, jobs, events, policies, migrations, factories, Blade-adjacent backend code, code review, and architecture decisions. Trigger whenever building, modifying, or reviewing ordinary Laravel application code."
license: UNLICENSED
---

# Laravel 13 Core

## Workflow

1. Inspect `composer.json`, Laravel version constraints, PHP constraint, and neighboring files.
2. Identify the slice: HTTP, model/domain, persistence, async, event, console, or integration.
3. Follow the application's existing conventions first. Apply these defaults only where no local pattern is clear.
4. Design from the framework boundary inward: route/request -> policy -> controller/action/service -> model/query -> resource/response -> test.
5. Validate with the smallest useful command set from `reference.md`.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Keep instructions **tool-agnostic**: "search/read/run" rather than IDE-specific actions.
- Prefer **thin, reviewable diffs** over large refactors.
- Always make **validation, authorization, and performance** explicit in the workflow for shared code paths.

## Defaults

- Target Laravel 13 and PHP 8.3+.
- Add `declare(strict_types=1);` to new PHP files when the project uses it.
- Follow accepted PHP-FIG PSRs where relevant: PSR-1 for basic PHP code, PSR-4 for autoloading, PSR-12 via Pint, PSR-11/14/20 at package or integration boundaries.
- Type parameters and return values.
- Keep controllers thin: validate/authorize, call an action/service when behavior is non-trivial, return a response/resource/redirect.
- Use Form Requests for validation and request authorization.
- Use Policies for authorization decisions.
- Keep Eloquent models focused on relationships, casts, scopes, accessors/mutators, factories, and domain-safe helpers.
- Prefer Laravel conventions over custom abstraction. Add repositories only for external persistence boundaries or when the project already uses them.
- Use transactions for multi-write invariants.
- Use queues for slow work after the response unless the user flow requires synchronous execution.

## Clean Architecture Defaults (when no local convention exists)

- Keep **domain decisions** close to the application layer (actions/services), not spread across controllers/views.
- Prefer **DTOs/value objects** for non-trivial inputs/outputs (especially across async/jobs/integrations).
- Avoid "helper soup": do not introduce global helpers for business rules.

## Stability Defaults (correctness under failure)

- Wrap multi-write invariants in a transaction; keep external I/O (HTTP, mail, broadcasts) outside it and dispatch after commit.
- Make jobs and event listeners idempotent; retries and at-least-once delivery must not double-apply effects.
- Retry transient database failures (deadlock, lock-wait) with capped backoff; do not retry non-idempotent work blindly.
- Configure jobs intentionally: `tries`, `backoff`, `timeout`, `failed()` handling; avoid silent swallowing of exceptions.
- Validate at the boundary and fail fast; return safe, non-leaking errors and let the framework handler log details.
- Avoid partial writes: order operations so a failure leaves a consistent state, or compensate explicitly.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
