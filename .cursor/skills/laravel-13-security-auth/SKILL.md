---
name: laravel-13-security-auth
description: Use for Laravel 13 security, OWASP review, authentication, authorization, Sanctum, Passport, policies, gates, Form Request authorize methods, session/token handling, validation hardening, mass assignment, file upload security, secrets, PII, and sensitive data handling.
---

# Laravel 13 Security and Auth

## Workflow

1. Identify the protected asset: account, tenant, payment, file, token, PII, admin action, or API resource.
2. Trace the request from route middleware through authentication, authorization, validation, persistence, response, and logs.
3. Check direct object references and tenant boundaries before polishing code.
4. Validate both allowed and denied behavior with tests when possible.

## Defaults

- Use Policies/Gates for authorization.
- Put request-specific authorization in Form Request `authorize()` when project style supports it.
- Use Sanctum for simple first-party or token API auth; Passport only when OAuth2 is required or already present.
- Never trust route model binding alone for ownership.
- Protect mass assignment with fillable/guarding conventions.
- Validate file uploads by MIME, size, extension expectations, storage disk, visibility, and authorization.
- Never log secrets, tokens, passwords, raw PII payloads, or payment data.
- Use encrypted casts or secure storage for sensitive fields when persistence is required.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
