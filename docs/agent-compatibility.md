# Agent Compatibility (Cursor, Codex, Claude Code)

This repository contains **agent skills** intended to be usable across different coding agents.
While Cursor Skills are the primary format, the guidance below keeps the instructions effective in:

- Cursor Agent / Cursor IDE
- Codex-style coding agents
- Claude Code / Claude CLI agents

## Compatibility Principles

- **Tool-agnostic language**: prefer "search the codebase", "read the file", "run tests" over naming IDE-specific tools.
- **Deterministic steps**: each workflow should be executable without proprietary features.
- **Small, composable slices**: prefer thin steps that can be applied incrementally.
- **Local conventions first**: always inspect nearby files, `composer.json`, routes, tests, and existing patterns before introducing new ones.
- **Explicit boundaries**: make security, authorization, validation, and performance checks a first-class step, not an afterthought.

## Baseline Quality Gates (agent-independent)

Run the smallest relevant set of commands that exist in the target project:

```bash
php artisan test
php artisan test --filter=SpecificTest
vendor/bin/pest
vendor/bin/phpunit
vendor/bin/pint
vendor/bin/phpstan analyse
composer test
composer lint
composer analyse
php artisan route:list
php artisan config:show app
```

## Output Expectations (recommended)

When implementing changes, prefer outputs that are easy for any agent to verify:

- A clear description of changed behavior (what/why).
- Tests added/updated for risky paths.
- Security/authorization notes (what is protected, and how).
- Performance notes for hot paths (query count, eager loading, pagination choice, caching strategy).

