# Coding Standards

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Overview

These coding standards define the expectations for all software written in this organization. Consistent code is easier to read, review, debug, and maintain. Every contributor is expected to follow these standards regardless of language or project.

---

## General Principles

1. **Clarity over cleverness** — Write code that a new team member can understand without explanation.
2. **Single Responsibility** — Functions and classes should do one thing well.
3. **Fail fast** — Validate inputs early and surface errors as close to the source as possible.
4. **No magic numbers** — Use named constants instead of unexplained literal values.
5. **DRY (Don't Repeat Yourself)** — Abstract repeated logic into shared utilities, but avoid premature abstraction.

---

## Naming Conventions

| Context | Style | Example |
|---|---|---|
| Variables / functions | `camelCase` | `getUserById` |
| Classes / types | `PascalCase` | `UserRepository` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| File names | `kebab-case` | `user-repository.ts` |
| Database columns | `snake_case` | `created_at` |

- Names must be descriptive and intention-revealing.
- Avoid abbreviations unless universally understood (e.g., `id`, `url`, `http`).
- Boolean variables and functions should read as predicates: `isActive`, `hasPermission`, `canRetry`.

---

## File & Module Organization

- One primary export per file unless the file is an index/barrel.
- Group imports: external packages → internal packages → relative imports, separated by blank lines.
- Keep files under 400 lines. Refactor when approaching this limit.
- Co-locate tests with the code they test (e.g., `user.service.ts` / `user.service.test.ts`).

---

## Functions & Methods

- Maximum function length: **50 lines** (prefer shorter).
- Maximum parameter count: **4**. Use an options object when more parameters are needed.
- Prefer pure functions (no side effects, same input → same output).
- Always return a value or throw explicitly; avoid implicit `undefined` returns in typed languages.
- Async functions must handle errors (try/catch or `.catch()`).

---

## Error Handling

- Never swallow exceptions silently. Log or re-throw.
- Use typed/structured errors where the language supports it.
- Provide actionable error messages: include what failed, why, and ideally how to fix it.
- Distinguish between user errors (4xx) and system errors (5xx) in APIs.

---

## Comments & Documentation

- Comment *why*, not *what*. Code should explain itself; comments explain intent.
- All public APIs must have doc-comments (JSDoc, GoDoc, Python docstrings, etc.).
- Remove commented-out code before merging. Use version control history instead.
- TODO/FIXME comments must reference a ticket: `// TODO(PROJ-123): remove after migration`.

---

## Testing Requirements

- All new code must have unit tests.
- Minimum coverage threshold: **80%** line coverage on new code.
- Tests must be deterministic — no flaky tests.
- Test names must describe the scenario: `it('returns 404 when user does not exist')`.
- Do not test implementation details; test observable behavior.

---

## Code Review

All code must pass review before merging. See the [Code Review Policy](../policies/code-review-policy.md) for full details.

---

## Enforcement

- Linters and formatters are configured in CI and must pass before merge.
- PRs that violate these standards will be blocked by reviewers.
- Persistent violations should be escalated to the team lead.

---

## Related Documents

- [Security Standards](./security-standards.md)
- [Documentation Standards](./documentation-standards.md)
- [Code Review Policy](../policies/code-review-policy.md)
