# Laravel 13 Skills Starter

## PT-BR

Base de projeto com skills para desenvolvimento de aplicações Laravel 13 com PHP 8.3+, APIs, testes, segurança, performance, Inertia, Laravel Boost, MCP e upgrades com Rector.

### Objetivo

Este repositório não é uma aplicação Laravel final. Ele é uma base de trabalho para agentes de IA:

- governança do agente
- skills especializadas
- convenções de arquitetura Laravel 13
- padrões de API, dados, segurança e testes
- padrões de frontend Laravel com Inertia
- padrões de IA com Laravel Boost, AI SDK e MCP

### Estrutura

- `AGENTS.md`: governança global do agente
- `.cursor/skills/*/SKILL.md`: skills especializadas
- `.cursor/skills/*/reference.md`: referências detalhadas por skill
- `docs/laravel-guidelines.md`: diretrizes do projeto

### Compatibilidade (Claude, Cursor e Codex)

- Este pacote foi escrito para uso multiagente.
- `AGENTS.md` contém regras gerais de comportamento e entrega.
- Os arquivos `.cursor/skills/*/SKILL.md` podem ser usados:
  - diretamente no Cursor, ou
  - como playbooks manuais no Claude/Codex, copiando ou referenciando o conteúdo relevante.
- Ao usar Claude/Codex, priorize:
  1. `AGENTS.md`
  2. a skill específica do tema
  3. `laravel-13-core` para contexto base

### Skills Disponíveis

#### Base Laravel

- `laravel-13-core`: controllers, models, rotas, Form Requests, actions/services, jobs, events, policies, migrations, factories e arquitetura.
- `laravel-13-api`: APIs REST, Resources, versionamento, paginação, Problem Details, Sanctum e testes de contrato.
- `laravel-13-data-performance`: Eloquent, N+1, índices, migrations seguras, cache, filas, Horizon, transações e grandes volumes.
- `laravel-13-testing-quality`: Pest/PHPUnit, factories, fakes, feature tests, Pint, Larastan/PHPStan, revisão e readiness.

#### Segurança e Interfaces

- `laravel-13-security-auth`: autenticação, autorização, policies, OWASP, uploads, mass assignment, secrets e dados sensíveis.
- `laravel-13-frontend-inertia`: Inertia, React, TypeScript, Tailwind, formulários, shared props, SSR, acessibilidade e realtime UI.

#### IA e Upgrade

- `laravel-13-ai-boost-mcp`: Laravel Boost, Laravel AI SDK, MCP tools/prompts/resources, embeddings, vector search e testes com fakes.
- `laravel-13-upgrade-rector`: upgrades para Laravel 13 com Rector, `driftingly/rector-laravel`, Composer e checklists de breaking changes.

### Quando Usar Cada Skill

- **Core Laravel**: sempre que houver código backend Laravel comum.
- **API**: endpoints, contratos JSON, Resources, autenticação de API, versionamento e erros.
- **Data/Performance**: queries lentas, N+1, migrations, índices, cache, jobs, transações ou grandes volumes.
- **Testing/Quality**: ao escrever testes, revisar código ou preparar merge/release.
- **Security/Auth**: sessões, tokens, policies, tenants, permissões, uploads, PII ou dados sensíveis.
- **Frontend Inertia**: telas Laravel com Inertia, React, Tailwind, formulários e estados de UI.
- **AI/Boost/MCP**: Laravel Boost, AI SDK, agentes, ferramentas MCP e busca vetorial.
- **Upgrade/Rector**: migrações de versão, Laravel 13, PHP 8.3+, Composer e automação com Rector.

### Como Usar

1. Abra esta pasta no seu ambiente de desenvolvimento.
2. Mantenha `AGENTS.md` na raiz do repositório.
3. Mantenha as skills em `.cursor/skills/`.
4. Peça tarefas ao agente citando claramente o contexto do módulo.
5. Para geração de código, informe sempre:
   - objetivo da feature
   - rota/API envolvida
   - regra de negócio
   - modelo de dados
   - perfil de usuário/permissão
   - requisito de teste

### Fluxo Recomendado

1. Identificar domínio e versão Laravel/PHP.
2. Escolher a skill principal.
3. Ler o `reference.md` da skill quando a tarefa precisar de detalhe.
4. Implementar seguindo as convenções locais do projeto alvo.
5. Validar com testes, Pint e análise estática quando disponíveis.
6. Registrar riscos e próximos passos.

### Exemplo de Uso no Claude/Codex

Prompt sugerido:

```text
Siga o arquivo AGENTS.md deste projeto e use como base as skills:
- laravel-13-core
- laravel-13-api
- laravel-13-testing-quality

Tarefa:
<descreva aqui a feature>
```

---

## EN

Project starter with skills for Laravel 13 application development with PHP 8.3+, APIs, tests, security, performance, Inertia, Laravel Boost, MCP, and Rector-based upgrades.

### Purpose

This repository is not a final Laravel application. It is an AI-agent working base for:

- agent governance
- specialized skills
- Laravel 13 architecture conventions
- API, data, security, and testing patterns
- Laravel frontend patterns with Inertia
- AI patterns with Laravel Boost, AI SDK, and MCP

### Structure

- `AGENTS.md`: global agent governance
- `.cursor/skills/*/SKILL.md`: specialized skills
- `.cursor/skills/*/reference.md`: detailed references for each skill
- `docs/laravel-guidelines.md`: project guidelines

### Compatibility (Claude, Cursor, and Codex)

- This package is written for multi-agent usage.
- `AGENTS.md` contains general behavior and delivery rules.
- `.cursor/skills/*/SKILL.md` files can be used:
  - directly in Cursor, or
  - as manual playbooks in Claude/Codex by copying or referencing the relevant content.
- When using Claude/Codex, prioritize:
  1. `AGENTS.md`
  2. the topic-specific skill
  3. `laravel-13-core` for baseline context

### Available Skills

#### Laravel Base

- `laravel-13-core`: controllers, models, routes, Form Requests, actions/services, jobs, events, policies, migrations, factories, and architecture.
- `laravel-13-api`: REST APIs, Resources, versioning, pagination, Problem Details, Sanctum, and contract tests.
- `laravel-13-data-performance`: Eloquent, N+1 issues, indexes, safe migrations, cache, queues, Horizon, transactions, and large datasets.
- `laravel-13-testing-quality`: Pest/PHPUnit, factories, fakes, feature tests, Pint, Larastan/PHPStan, review, and readiness.

#### Security and Interfaces

- `laravel-13-security-auth`: authentication, authorization, policies, OWASP, uploads, mass assignment, secrets, and sensitive data.
- `laravel-13-frontend-inertia`: Inertia, React, TypeScript, Tailwind, forms, shared props, SSR, accessibility, and realtime UI.

#### AI and Upgrade

- `laravel-13-ai-boost-mcp`: Laravel Boost, Laravel AI SDK, MCP tools/prompts/resources, embeddings, vector search, and tests with fakes.
- `laravel-13-upgrade-rector`: Laravel 13 upgrades with Rector, `driftingly/rector-laravel`, Composer, and breaking-change checklists.

### When To Use Each Skill

- **Core Laravel**: whenever ordinary Laravel backend code is involved.
- **API**: endpoints, JSON contracts, Resources, API authentication, versioning, and errors.
- **Data/Performance**: slow queries, N+1 issues, migrations, indexes, cache, jobs, transactions, or large datasets.
- **Testing/Quality**: when writing tests, reviewing code, or preparing for merge/release.
- **Security/Auth**: sessions, tokens, policies, tenants, permissions, uploads, PII, or sensitive data.
- **Frontend Inertia**: Laravel screens with Inertia, React, Tailwind, forms, and UI states.
- **AI/Boost/MCP**: Laravel Boost, AI SDK, agents, MCP tools, and vector search.
- **Upgrade/Rector**: version migrations, Laravel 13, PHP 8.3+, Composer, and Rector automation.

### How To Use

1. Open this folder in your development environment.
2. Keep `AGENTS.md` at the repository root.
3. Keep skills under `.cursor/skills/`.
4. Ask the agent for tasks while clearly naming the module context.
5. For code generation, always provide:
   - feature goal
   - route/API involved
   - business rule
   - data model
   - user role/permission
   - test requirement

### Recommended Flow

1. Identify domain and Laravel/PHP version.
2. Choose the primary skill.
3. Read the skill's `reference.md` when the task needs detail.
4. Implement following the target project's local conventions.
5. Validate with tests, Pint, and static analysis when available.
6. Record risks and next steps.

### Claude/Codex Usage Example

Suggested prompt:

```text
Follow this project's AGENTS.md file and use these skills:
- laravel-13-core
- laravel-13-api
- laravel-13-testing-quality

Task:
<describe the feature here>
```

