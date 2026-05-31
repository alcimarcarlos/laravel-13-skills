# laravel-13-security-auth Reference

## PHP-FIG PSR Touchpoints

- PSR-3 log context must not include secrets, tokens, raw PII, payment data, or credentials.
- PSR-7 request/response objects can still carry sensitive headers, uploaded files, streams, and bodies; review them like Laravel requests.
- PSR-15 middleware/request handlers must enforce the same authentication, authorization, CSRF, tenant, and rate-limit expectations as Laravel middleware.
- PSR-18 clients should define safe timeouts, avoid SSRF-prone unvalidated URLs, and map provider errors without leaking internals.

## AuthN + AuthZ

## Authentication

- Session auth for browser apps.
- Sanctum for SPA auth and personal access token APIs.
- Passport for OAuth2 provider needs.
- Custom guards only when framework defaults cannot model the boundary.

## Authorization

Check:

- Route middleware.
- Policy method for the model/action.
- Tenant/account ownership constraints.
- Admin bypasses are explicit and tested.
- Query scopes do not leak cross-tenant records.

## Tests

Add tests for:

- Guest denied.
- Authenticated wrong owner denied.
- Correct owner allowed.
- Admin or elevated role allowed/denied according to policy.
- API token missing ability/scope denied.

## OWASP + Sensitive Data

Review for:

- Broken access control.
- Injection through raw SQL, unescaped output, unsafe query fragments.
- Authentication/session weaknesses.
- Insecure direct object references.
- SSRF via user-supplied URLs.
- Unsafe file upload and public storage.
- Mass assignment.
- Sensitive data exposure in responses, logs, queues, broadcasts, notifications, or exception reports.

Hardening defaults:

- Validate and normalize input.
- Escape output in Blade; only use raw output for trusted HTML.
- Prefer query builder bindings over raw string interpolation.
- Use signed URLs for temporary private file access.
- Redact sensitive data in logs and observability tools.
- Rotate leaked keys immediately; do not commit `.env` secrets.

## Rate Limiting and Abuse Control

- Define named limiters (for example in a service provider) and apply `throttle:<name>` middleware to sensitive routes.
- Key login/OTP/password-reset limiters by credential + IP to slow credential stuffing without locking out shared NATs entirely.
- Apply a default API throttle keyed by token/user; tighten it for write or expensive endpoints.
- Bound list endpoints with a maximum page size and reject oversized `per_page` values.
- Return `429` with `Retry-After` when throttled; never leak whether an account exists in auth error messages.

## Transport, Cookies, and Headers

- Enforce HTTPS in production and emit HSTS only over HTTPS.
- Session config: `secure=true` (prod), `http_only=true`, `same_site=lax|strict`, encrypted cookies enabled.
- Regenerate session ID on login and invalidate it on logout; rotate tokens on privilege change.
- Baseline response headers: `X-Content-Type-Options: nosniff`, frame protection or CSP, `Referrer-Policy`, and a minimal CSP for first-party assets.
- Restrict CORS to known origins, methods, and headers; do not reflect arbitrary origins with credentials enabled.

## Injection-Safe Queries

- Allowlist sortable/filterable columns and directions; map request keys to a fixed set before querying.
- Never interpolate request data into `orderByRaw`, `whereRaw`, `selectRaw`, `havingRaw`, or `DB::raw`.
- Use bound parameters when raw expressions are truly required.

## Secrets and Supply Chain

- Keep secrets in environment/secret managers, never in code, fixtures, or VCS history.
- Pin and audit dependencies; review lockfile changes and known-vulnerability advisories.
- Scrub secrets/PII from exception reports and third-party error/observability tools.
- On any leak, rotate the credential and invalidate dependent tokens/sessions immediately.

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
