---
name: laravel-13-ai-boost-mcp
description: Use for Laravel 13 AI-assisted development, Laravel Boost, Laravel AI SDK, laravel/ai, MCP server development, MCP tools/prompts/resources, AI agents, structured output, embeddings, vector search, reranking, image/audio generation, AI fakes in tests, and Boost search-docs workflows.
---

# Laravel 13 AI, Boost, and MCP

## Workflow

1. Check whether Laravel Boost is installed and available.
2. Use Boost/search-docs or official package docs for exact APIs when available.
3. Identify whether the task is app AI feature, AI SDK integration, MCP server/tool, or agent workflow.
4. Treat AI boundaries like external services: validate inputs, constrain outputs, fake in tests, and avoid leaking sensitive data.
5. Test deterministic behavior around prompts, tools, schema validation, and fallback paths.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Treat "Boost" as optional. If unavailable, rely on `composer.json` + official docs and avoid guessing fast-moving APIs.
- Keep MCP tool specs **self-describing** (name, purpose, input schema, output shape) so they work in any agent runtime.

## Defaults

- Prefer official Laravel packages and patterns over hand-rolled clients.
- Use structured output schemas when downstream code depends on AI output.
- Keep prompts versioned and close to the feature.
- Use fakes/mocks for AI providers in tests.
- For MCP tools, define clear input schemas, narrow capabilities, authorization, and safe error messages.
- For embeddings/vector search, document model, dimensions, storage, indexing, and refresh strategy.
- For package-style AI/MCP integrations, prefer PSR-3 logging, PSR-18 HTTP clients, PSR-20 clocks, and PSR-11 container boundaries when interoperability is useful.

## Security + Cost Controls (default stance)

- Never pass secrets/PII to providers unless strictly necessary and permitted.
- Add guardrails against **prompt injection** and **tool overreach** (narrow tools, validate inputs, validate outputs).
- Bound execution: max tokens, max tool calls, timeouts, retries, and budget controls when available.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
