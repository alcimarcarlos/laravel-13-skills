# laravel-13-api Reference

## problem details

Use RFC 9457-style problem documents for JSON API errors when the project uses or wants a consistent error contract.

Recommended fields:

- `type`: stable URI or slug for the problem kind.
- `title`: short human-readable summary.
- `status`: HTTP status code.
- `detail`: specific message for this occurrence.
- `instance`: request path or traceable occurrence URI when available.
- `errors`: validation field errors for `422` responses.

Common statuses:

- `400` invalid request shape.
- `401` unauthenticated.
- `403` authenticated but forbidden.
- `404` missing resource.
- `409` state conflict.
- `422` validation failed.
- `429` rate limited.

Do not expose exception internals, SQL, tokens, stack traces, or sensitive identifiers in production API errors.

## rest patterns

## Route Shape

- Group routes by resource or bounded context.
- Apply auth and throttle middleware explicitly.
- Name routes when clients, tests, redirects, or generated links need them.

## Controllers

Acceptable styles:

- Invokable controller per action for strict API codebases.
- Resource controller when already established and methods stay thin.

Controller responsibilities:

- Receive a Form Request.
- Call action/service/model query.
- Return Resource, ResourceCollection, `JsonResponse`, or empty response.

## Resources

- Keep response shape stable.
- Include relationships only when loaded or explicitly requested.
- Avoid leaking internal columns by returning raw models.

## Async Work

- For slow side effects, create state, dispatch a job, return `202 Accepted`.
- Include a status URL or job/resource identifier when useful.
- Make jobs idempotent and retry-safe.

## versioning

Use versioning when clients need stable contracts across releases.

Options:

- URI versioning: `/v1/posts`.
- Header versioning: useful for strict API platforms, but harder to test manually.
- Route-file organization: `routes/api/v1/posts.php` for larger APIs.

Deprecation:

- Announce removal dates in documentation.
- Add `Sunset` and `Deprecation` headers when deprecating versions.
- Keep old behavior covered by tests until removal.

Breaking changes:

- Removing or renaming fields.
- Changing enum values.
- Changing pagination structure.
- Changing status codes or error shapes.
- Tightening validation in a way existing clients may fail.
