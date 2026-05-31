# laravel-13-api Reference

## PHP-FIG PSR Touchpoints

- PSR-7: use only when a package, bridge, webhook SDK, or middleware stack expects immutable HTTP messages.
- PSR-15: use for PSR request handlers/middleware. Do not replace normal Laravel middleware unless the integration requires it.
- PSR-17: use factories when creating PSR-7 requests, responses, streams, or uploaded files.
- PSR-18: use `Psr\Http\Client\ClientInterface` for portable API clients or package-like SDK boundaries. Prefer Laravel's `Http` client for ordinary app calls.
- PSR-13: use typed link relations for hypermedia-heavy API packages; plain resource links are fine for normal Laravel APIs.
- Keep API error documents compatible with RFC 9457 when adopted; this is separate from PHP-FIG PSRs.

## Problem Details

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

## REST Patterns

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

## Idempotency and Retries

- Accept an idempotency key on unsafe, retriable endpoints; persist the first result keyed by client + key and replay it on duplicate requests.
- Use unique constraints or status checks so concurrent duplicates cannot create double records.
- Document which endpoints are safe to retry and the expected client backoff behavior.

## Resilience to Upstreams

- Wrap outbound calls with explicit timeouts and capped, jittered retries; never retry non-idempotent upstream writes blindly.
- Consider a circuit breaker or short-circuit fallback when a dependency is failing, to avoid cascading slowdowns.
- Map upstream/internal failures to stable status codes (`502/503/504`) with a safe Problem Details body; never surface stack traces, SQL, or secrets.
- Send `Retry-After` with `429`/`503` so clients can back off deterministically.

## Versioning

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

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
