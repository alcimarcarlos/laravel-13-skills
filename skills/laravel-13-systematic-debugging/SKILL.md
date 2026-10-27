---
name: laravel-13-systematic-debugging
description: "Laravel 13 systematic debugging: minimal Pest test reproducers, single-variable fixes. Trigger when diagnosing bugs."
license: UNLICENSED
---

# Laravel 13 Systematic Debugging

## Objective

Eliminate guesswork ("shotgun debugging") and prevent regression loops by enforcing a disciplined 4-step scientific method for triaging and resolving bugs in Laravel applications.

## Compatibility (Cursor, Codex, Claude Code)

- Strict operational gate: agents must not alter production source code until a minimal failing test reproducer is written.
- Always isolate the root cause before implementing the solution.

## Mandatory 4-Step Protocol

### Step 1: Formulate Root-Cause Hypothesis
- Inspect the stack trace, error logs (`storage/logs/laravel.log`), and exception details.
- Formulate an explicit hypothesis: *"The bug occurs because class X passes input Y without casting, resulting in Z."*

### Step 2: Create Minimal Failing Reproducer
- Create or add a test in `tests/Feature/` or `tests/Unit/` that directly triggers the reported scenario.
- Run the test:
  ```bash
  php artisan test --filter=BugReproducerTest
  ```
- **Verification Gate**: The test MUST fail with the exact reported error. If it passes, the hypothesis is wrong or incomplete.

### Step 3: Surgical Fix (Single Variable)
- Make the smallest possible change to resolve the root cause.
- Follow the **Minimal Diff Mandate**: never rewrite unaffected methods, never add unrelated dependencies.

### Step 4: Non-Regression Verification
- Re-run the reproducer test to verify it passes:
  ```bash
  php artisan test --filter=BugReproducerTest
  ```
- Run the full test suite and code style linter:
  ```bash
  php artisan test
  vendor/bin/pint --test
  ```

## Anti-Patterns (Strictly Forbidden)

- **Shotgun Debugging**: Guessing multiple changes at once and hoping the error disappears.
- **Error Silencing**: Wrapping broken code in empty `try { ... } catch (\Throwable $e) {}`.
- **Modifying Assertions**: Changing a test assertion to match incorrect code output instead of fixing the root cause.

## Additional Resources

- Read `reference.md` for triage commands and reproducer templates.
