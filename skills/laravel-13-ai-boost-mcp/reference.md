# laravel-13-ai-boost-mcp Reference

## PHP-FIG PSR Touchpoints

- PSR-3: inject `LoggerInterface` for provider/tool telemetry, with redacted context.
- PSR-18: use `ClientInterface` for portable provider clients or SDK-like adapters; Laravel `Http` remains fine for application-local calls.
- PSR-20: inject clocks for rate limits, token budgets, retries, transcript retention, and deterministic tests.
- PSR-11: expose container dependencies only at package/framework boundaries; prefer explicit constructor dependencies inside application services.
- PSR-6/16: use interoperable cache interfaces for reusable embeddings, model metadata, or provider cache packages; use Laravel cache for ordinary app storage.

## AI SDK

Patterns:

- Wrap provider calls behind application services/actions.
- Use typed DTOs or structured output for consumed responses.
- Validate user-provided instructions before passing them to tools.
- Store transcripts, prompts, outputs, embeddings, and metadata only when needed and permitted.
- Use queues for long-running generation or embedding jobs.
- Fake AI calls in tests and assert the request shape.

Risk checks:

- Prompt injection.
- Tool overreach.
- Sensitive data in prompts.
- Unbounded token/cost growth.
- Non-deterministic output used as authority without validation.

## Boost

Use Boost when available to:

- Search version-specific Laravel documentation.
- Inspect routes, config, database, logs, and app context through MCP tools.
- Keep generated code aligned with the installed Laravel/package versions.

When Boost is not installed:

- Inspect `composer.json`.
- Prefer official Laravel 13 docs for APIs.
- Avoid guessing APIs for newly released packages.

## MCP

For MCP tools:

- Define a narrow name and description.
- Specify input schema and validate it.
- Authorize access to application data.
- Return concise, structured output.
- Avoid raw exception messages.

For MCP resources:

- Expose read-only, bounded context.
- Avoid secrets, tokens, or raw PII.
- Paginate or summarize large resources.

For MCP prompts:

- Keep task instructions focused.
- Include required inputs and expected output shape.
- Avoid embedding environment-specific secrets.

Testing:

- Test schema validation.
- Test authorization denial.
- Test success output shape.
- Test provider/app failure behavior.

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
