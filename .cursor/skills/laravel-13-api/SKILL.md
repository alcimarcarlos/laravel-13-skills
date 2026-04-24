---
name: laravel-13-api
description: Use for Laravel 13 REST API design and implementation, including API routes, resources, JSON responses, RFC 9457 Problem Details, versioning, pagination, Sanctum token auth, policies, Form Requests, OpenAPI-style contracts, async 202 workflows, and API tests.
---

# Laravel 13 API

## Workflow

1. Read existing `routes/api*.php`, API Resources, Form Requests, policies, and tests.
2. Decide the contract before code: endpoint, method, auth, request body, response status, resource shape, errors, pagination, and version behavior.
3. Put validation and authorization at the edge with Form Requests and Policies.
4. Serialize with Resources, not raw models.
5. Test success, validation failure, authorization failure, not found, and pagination/version behavior when applicable.

## API Defaults

- Use consistent route grouping and naming.
- Prefer `simplePaginate()` or cursor pagination for large public lists unless UX requires total counts.
- Use API Resources for response shape.
- Use `201 Created` for create, `202 Accepted` for queued/background work, `204 No Content` for successful deletes with no body.
- Use Sanctum for first-party/token API auth unless the project already uses Passport or another provider.
- Use Problem Details style errors when the project accepts opinionated API contracts. See `reference.md`.
- Keep API versioning explicit and documented. See `reference.md`.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
