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
| `laravel-13-ai-boost-mcp` | Laravel 13 AI & MCP: Boost, AI SDK, agents, vector search, MCP tools. Trigger on AI integrations. |
| `laravel-13-api` | Laravel 13 REST APIs: routes, RFC 9457 errors, Sanctum, pagination, resources. Trigger on API tasks. |
| `laravel-13-code-generation-contract` | Mandatory output contract for Laravel 13: file trees, FormRequests, policies, tests. Trigger on new backend slices. |
| `laravel-13-core` | Laravel 13 backend: controllers, FormRequests, policies, Eloquent, actions, jobs. Trigger on ordinary Laravel tasks. |
| `laravel-13-data-performance` | Laravel 13 data performance: N+1 prevention, chunking, indexes, transactions. Trigger on query or DB optimization. |
| `laravel-13-frontend-inertia` | Laravel 13 Inertia frontend: React, TypeScript, forms, shared props, SSR. Trigger on Inertia UI tasks. |
| `laravel-13-security-auth` | Laravel 13 security: Sanctum, policies, mass assignment, secrets, PII, CSRF. Trigger on auth or security work. |
| `laravel-13-systematic-debugging` | Laravel 13 systematic debugging: minimal Pest test reproducers, single-variable fixes. Trigger when diagnosing bugs. |
| `laravel-13-testing-quality` | Laravel 13 test quality: Pest 4, PHPUnit 12, feature/unit tests, Pint, quality gates. Trigger when writing tests. |
| `laravel-13-upgrade-rector` | Laravel 13 upgrade with Rector: PHP 8.3+, package conflicts, and migration rules. Trigger on version upgrades. |
| `laravel-13-zero-downtime-migrations` | Zero-downtime migrations in Laravel: Expand/Contract, concurrent indexes, backfills. Trigger on DB schema changes. |
| `mysql-rds-optimization` | MySQL 8+ and AWS RDS/Aurora: connection pooling, RDS Proxy, gap lock/deadlock prevention, indexes. Trigger on DB tuning. |
| `redis-architecture-caching` | Redis & ElastiCache architecture: eviction safety, SCAN vs KEYS, stampede locks, pipelines. Trigger on caching/queues. |

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
