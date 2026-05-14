# laravel-13-frontend-inertia Reference

## PHP-FIG PSR Touchpoints

- PHP-side controllers, Form Requests, resources, and shared-prop providers should follow PSR-1, PSR-4, and PSR-12/Pint conventions.
- Frontend TypeScript, React/Vue/Svelte, Tailwind, and Vite code follows the project's JavaScript formatter and lint rules, not PHP-FIG PSRs.
- If an Inertia page uses server-side HTTP integrations, apply the API skill's PSR-7/17/18 guidance at that boundary.

## Inertia Forms

## Page Props

- Pass only data the page needs.
- Transform models through resources/arrays.
- Avoid sending secrets, hidden columns, or full unrelated relations.

## Forms

- Let Laravel validation own the rules.
- Display field errors next to inputs.
- Preserve scroll and state where it improves correction flow.
- Disable or show pending state during submission.
- Handle upload progress for file forms.

## Navigation

- Use project route helpers if present.
- Prefer named routes for server-generated links.
- Use partial reloads for large pages where appropriate.

## UI + Realtime

## Accessibility

- Use semantic elements and labels.
- Ensure keyboard navigation and visible focus.
- Keep contrast readable.
- Do not rely on color alone for status.
- Announce validation and async status changes where practical.

## State Coverage

Cover:

- Loading.
- Success.
- Empty.
- Validation error.
- Server error.
- Unauthorized/forbidden.
- Offline or retryable network failure when relevant.

## Realtime

- Use Echo/Reverb only after confirming broadcast config.
- Authorize private channels.
- Avoid broadcasting sensitive payloads.
- Reconcile realtime updates with pagination and filters.

## Agent Compatibility Notes

For cross-agent usage guidance, see `docs/agent-compatibility.md`.
