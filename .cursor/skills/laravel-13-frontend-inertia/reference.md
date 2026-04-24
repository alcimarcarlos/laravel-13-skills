# laravel-13-frontend-inertia Reference

## inertia forms

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

## ui realtime

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
