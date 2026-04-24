# laravel-13-security-auth Reference

## authz

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

## owasp sensitive data

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
