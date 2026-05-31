---
name: laravel-13-api
description: "Use for Laravel 13 REST API design and implementation, including API routes, resources, JSON responses, RFC 9457 Problem Details, versioning, pagination, Sanctum token auth, policies, Form Requests, OpenAPI-style contracts, async 202 workflows, and API tests."
license: UNLICENSED
---

# Laravel 13 API

## Workflow

1. Read existing `routes/api*.php`, API Resources, Form Requests, policies, and tests.
2. Decide the contract before code: endpoint, method, auth, request body, response status, resource shape, errors, pagination, and version behavior.
3. Put validation and authorization at the edge with Form Requests and Policies.
4. Serialize with Resources, not raw models.
5. Test success, validation failure, authorization failure, not found, and pagination/version behavior when applicable.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Describe the **contract** in plain terms first (method, URL, auth, payload, response shape).
- Keep API guidance **framework-native** (Form Requests, Policies, Resources) to avoid agent-specific abstractions.

## API Defaults

- Use consistent route grouping and naming.
- Prefer `simplePaginate()` or cursor pagination for large public lists unless UX requires total counts.
- Use API Resources for response shape.
- Use `201 Created` for create, `202 Accepted` for queued/background work, `204 No Content` for successful deletes with no body.
- Use Sanctum for first-party/token API auth unless the project already uses Passport or another provider.
- Use Problem Details style errors when the project accepts opinionated API contracts. See `reference.md`.
- Keep API versioning explicit and documented. See `reference.md`.
- Use PSR-7/15/17/18 only at HTTP interoperability boundaries; keep ordinary Laravel controllers, middleware, Resources, and tests Laravel-native.

## Security + Performance Defaults

- Enforce **authz** at both query and policy levels (avoid cross-tenant leaks).
- Avoid N+1 and unbounded lists; use **explicit eager loading** and bounded pagination.
- Never leak exception internals or sensitive fields in API resources or error payloads.
- Apply `throttle` middleware to every public/auth endpoint; tighten limits for write and expensive routes.
- Enforce a maximum page size and reject oversized `per_page`/filter inputs.

## Stability Defaults (resilient contracts)

- Support idempotency for unsafe retried requests (for example an `Idempotency-Key` header) so a retried `POST` does not duplicate work.
- Set bounded timeouts and capped, jittered retries on upstream calls; map upstream failures to stable `502/503/504` without leaking internals.
- Degrade gracefully: return `503` with `Retry-After` when a dependency is down rather than hanging the request.
- Keep responses bounded (pagination, field limits) so a single client cannot exhaust memory or time.
- Return consistent error shapes for `429`/`5xx` so clients can implement safe backoff.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
