---
name: laravel-13-code-generation-contract
description: "Mandatory output contract for Laravel 13: file trees, FormRequests, policies, tests. Trigger on new backend slices."
license: UNLICENSED
---

# laravel-13-code-generation-contract

## Objective

Standardize the format, architectural boundaries, and minimum completeness of agent deliveries in Laravel 13 and PHP 8.3+.

## Compatibility (Cursor, Codex, Claude Code)

- Agent-vendor agnostic: treat as a mandatory delivery protocol.
- Prefer deterministic, reviewable outputs over conversational suggestions.
- Do not invent models, migrations, or third-party packages silently without declaring assumptions.

## Apply When

- Creating or refactoring an API endpoint, controller, or action.
- Generating domain services, jobs, event listeners, or policies.
- Implementing an entire feature slice from route to database.
- Reviewing PRs or checking whether a generated backend implementation is production-ready.

## Mandatory Delivery Protocol

Every backend feature delivery MUST provide:

1. **Declared File Tree**: Relative paths of all created/modified files before code blocks (`routes/`, `app/Http/Requests/`, `app/Policies/`, `app/Actions/`, `app/Models/`, `tests/Feature/`).
2. **Boundary Contracts**:
   - `FormRequest` for input validation and request-level authorization.
   - `Policy` for domain resource authorization.
   - `JsonResource` or typed DTO for external API output (RFC 9457 for errors).
3. **Thin Controller / Single Responsibility**: Controllers only coordinate HTTP parsing, policy check, action delegation, and response rendering. Business rules reside in Action/Service classes.
4. **Resilience & DB Safety**:
   - Multi-write operations wrapped in `DB::transaction()`.
   - External I/O (mail, webhook, queues) dispatched outside transactions (via `DB::afterCommit()`).
   - Idempotency guards on queueable jobs.
5. **Security Baseline**:
   - No `Model::unguard()` or Mass Assignment vulnerabilities.
   - Constant-time comparison (`hash_equals`) for tokens and signatures.
   - No sensitive data (passwords, tokens, raw PII) in logs or JSON error messages.
6. **Actionable Test Matrix (Pest 4 / PHPUnit 12)**:
   - Happy path (200/201).
   - Unauthenticated guest denied (401).
   - Authenticated user without ownership/permission denied (403).
   - Invalid payload fails with validation errors (422).
   - Boundary/edge failure handled gracefully (404/409/500).

## Mandatory Output Format

1. **Objective & Slice**: Clear summary of what is being built.
2. **File Structure**: Tree of files to create or modify.
3. **Contracts & Validation**: Form Requests, Policies, and DTOs.
4. **Implementation**: Actions/Services, Models, Migrations, and Controllers.
5. **Routes & Registration**: Route definitions and middleware bindings.
6. **Automated Tests**: Pest test suite covering the test matrix.
7. **Verification Commands**: Exact commands to validate locally (e.g. `php artisan test --filter=...`, `vendor/bin/pint`).

## Additional Resources

- For checklist templates and complete slice examples, read `reference.md`.
