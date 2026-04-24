---
name: laravel-13-frontend-inertia
description: Use for Laravel 13 frontend work with Inertia.js, React, TypeScript, Tailwind, Vite, Blade integration, forms, validation errors, shared props, SSR, accessibility, realtime UI, file uploads, and frontend tests in Laravel applications.
---

# Laravel 13 Frontend Inertia

## Workflow

1. Detect stack: Inertia React/Vue/Svelte, Blade, Livewire, Tailwind version, Vite, TypeScript.
2. Follow existing component layout, form helper, routing helper, and design token conventions.
3. Keep server truth in Laravel; keep client state local unless it improves UX.
4. Preserve validation, authorization, flash messages, loading, empty, error, offline, and unauthorized states.
5. Test critical flows through feature tests and frontend tests when configured.

## Defaults

- Use Inertia responses from controllers and typed page props where the project supports TypeScript.
- Use Form Requests and return Laravel validation errors into the Inertia form flow.
- Keep shared props minimal and non-sensitive.
- Use Tailwind utilities consistently with the app's version and design system.
- Ensure forms are keyboard accessible and errors are announced/visible.
- Use optimistic UI only when rollback behavior is clear.
- For realtime UI, prefer Laravel Reverb/Echo patterns already configured in the app.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
