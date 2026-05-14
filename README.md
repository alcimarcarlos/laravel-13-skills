# Laravel 13 Skills

## Objetivo

Skills para Laravel 13 com PHP 8.3+, APIs, Eloquent, segurança, testes, Inertia, performance, Laravel Boost, MCP e upgrades com Rector.

Este pacote não é uma aplicação final. Ele é uma coleção portátil de instruções para agentes de código trabalharem em projetos reais com padrões consistentes.

## Compatibilidade

| Agente | Entrada recomendada |
| --- | --- |
| Cursor | `.cursor/rules/skills.mdc` e aliases em `.cursor/skills/<skill-name>` |
| Codex | `skills/<skill-name>/SKILL.md` ou cópia/symlink em `$CODEX_HOME/skills` |
| Claude Code | `CLAUDE.md`, depois `skills/<skill-name>/SKILL.md` |
| GitHub Copilot | `.github/copilot-instructions.md`, apontando para `AGENTS.md` e `skills/` |

## Estrutura

- `AGENTS.md`: instruções operacionais para agentes.
- `CLAUDE.md`: ponto de entrada curto para Claude Code.
- `.cursor/rules/skills.mdc`: regra de descoberta para Cursor.
- `.cursor/skills/<skill-name>`: symlink para `skills/<skill-name>`.
- `.github/copilot-instructions.md`: instruções curtas para GitHub Copilot.
- `skills/<skill-name>/SKILL.md`: fonte canônica da skill.
- `skills/<skill-name>/reference.md`: detalhes carregados somente quando a skill pedir.

## Stack Coberta

- Laravel 13
- PHP 8.3+
- Eloquent
- Sanctum
- Inertia
- Pest/PHPUnit
- Rector

## Como Escolher Skills

1. Leia `AGENTS.md` para as regras gerais da coleção.
2. Escolha a menor skill que cobre a tarefa.
3. Use `laravel-13-core` como baseline quando a tarefa cruzar vários temas.
4. Leia `reference.md` apenas quando a skill ou a complexidade da tarefa pedir.
5. Valide com os comandos disponíveis no projeto alvo.

## Skills Disponíveis

| Skill | Quando usar |
| --- | --- |
| `laravel-13-ai-boost-mcp` | Use for Laravel 13 AI-assisted development, Laravel Boost, Laravel AI SDK, laravel/ai, MCP server development, MCP tools/prompts/resources, AI agents, structured output, embeddings, vector search, reranking, image/audio generation, AI fakes in tests, and Boost search-docs workflows. |
| `laravel-13-api` | Use for Laravel 13 REST API design and implementation, including API routes, resources, JSON responses, RFC 9457 Problem Details, versioning, pagination, Sanctum token auth, policies, Form Requests, OpenAPI-style contracts, async 202 workflows, and API tests. |
| `laravel-13-core` | Use for Laravel 13 backend development in PHP 8.3+, including controllers, models, routes, Form Requests, services/actions, jobs, events, policies, migrations, factories, Blade-adjacent backend code, code review, and architecture decisions. Trigger whenever building, modifying, or reviewing ordinary Laravel application code. |
| `laravel-13-data-performance` | Use for Laravel 13 database, Eloquent, caching, performance, queues, Horizon, transactions, indexing, N+1 fixes, chunking, cursor pagination, Redis/cache design, query optimization, large datasets, and zero-downtime migration concerns. |
| `laravel-13-frontend-inertia` | Use for Laravel 13 frontend work with Inertia.js, React, TypeScript, Tailwind, Vite, Blade integration, forms, validation errors, shared props, SSR, accessibility, realtime UI, file uploads, and frontend tests in Laravel applications. |
| `laravel-13-security-auth` | Use for Laravel 13 security, OWASP review, authentication, authorization, Sanctum, Passport, policies, gates, Form Request authorize methods, session/token handling, validation hardening, mass assignment, file upload security, secrets, PII, and sensitive data handling. |
| `laravel-13-testing-quality` | Use for Laravel 13 test writing, Pest 4, PHPUnit 12, factories, feature/unit tests, HTTP tests, database assertions, facade fakes, code review, Pint, Larastan/PHPStan, quality gates, CI readiness, and release validation. |
| `laravel-13-upgrade-rector` | Use when upgrading a Laravel application or package to Laravel 13, migrating Laravel versions, configuring Rector with driftingly/rector-laravel, checking PHP 8.3+ requirements, resolving composer conflicts, planning dependency upgrades, or producing a Laravel 13 upgrade checklist. |

## Qualidade e Validação

Execute o menor conjunto relevante que existir no projeto alvo:

- `php artisan test`
- `vendor/bin/pest`
- `vendor/bin/phpunit`
- `vendor/bin/pint`
- `vendor/bin/phpstan analyse`
- `composer test`

## Notas de Uso

- Prefira padrões nativos Laravel antes de abstrações novas.
- Use laravel-13-security-auth quando houver sessão, token, tenant, policy, upload, PII ou segredo.
- Aplique PSRs aceitas em pontos de interoperabilidade sem substituir convenções Laravel.

## Instalação por Symlink

Para Codex:

```bash
mkdir -p "$HOME/.codex/skills"
for d in skills/*; do
  name="$(basename "$d")"
  ln -sfn "$(pwd)/$d" "$HOME/.codex/skills/$name"
done
```

Para Claude Code:

```bash
mkdir -p "$HOME/.claude/skills"
for d in skills/*; do
  name="$(basename "$d")"
  ln -sfn "$(pwd)/$d" "$HOME/.claude/skills/$name"
done
```

Para Cursor em um projeto consumidor:

```bash
mkdir -p .cursor/skills
for d in skills/*; do
  name="$(basename "$d")"
  ln -sfn "../../skills/$name" ".cursor/skills/$name"
done
```

## Prompt Base

```text
Siga AGENTS.md, escolha a menor skill aplicável em skills/<nome>/SKILL.md e carregue reference.md somente se necessário.

Contexto:
- Stack: Laravel 13, PHP 8.3+, Eloquent, Sanctum, Inertia, Pest/PHPUnit, Rector
- Objetivo: <descreva a tarefa>
- Restrições: <auth, dados, UX, performance, compatibilidade>
- Validação esperada: <testes/build/lint>
```
