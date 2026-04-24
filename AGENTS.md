# Laravel 13 Agent Instructions

You are working with a skill package for Laravel 13 development.

## Principles

- Use Laravel 13 and PHP 8.3+ as the default reference unless the target project states otherwise.
- Read the target project's existing patterns before introducing new conventions.
- Use the smallest relevant skill for the task.
- For details, read the triggered skill's `reference.md`.
- Prefer native Laravel patterns over custom abstractions.
- Generate testable code with clear validation and low coupling.
- Do not create ceremonial architecture when a simple Laravel convention solves the problem.

## Skill Structure

- `.cursor/skills/<skill>/SKILL.md`: triggers, purpose, and main workflow.
- `.cursor/skills/<skill>/reference.md`: details, checklists, and specific patterns.
- `docs/laravel-guidelines.md`: general guidelines and source notes used during curation.

## Decision Order

1. Identify whether the task is backend, API, data/performance, testing, security, frontend Inertia, AI/MCP, or upgrade work.
2. Use the corresponding skill.
3. If the task crosses multiple contexts, use `laravel-13-core` as the base and add only the needed skills.
4. Before editing code in a real Laravel project, inspect neighboring files, `composer.json`, routes, tests, and local conventions.
5. When finished, report validation commands that ran or remained pending.

## Minimum Quality Bar

- Thin controllers.
- Form Requests for input validation/authorization.
- Policies/Gates for authorization.
- API Resources for structured JSON.
- Eager loading for relationships used in lists/loops.
- Transactions for multi-write invariants.
- Jobs for slow or retryable work.
- Tests for critical behavior.
- Pint and static analysis when configured.

