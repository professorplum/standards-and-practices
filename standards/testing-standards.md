# Testing Standards

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Overview

Testing is not optional. It is how we verify correctness, prevent regressions, and give engineers confidence to change code. These standards define what types of tests are required, how they should be written, and what coverage expectations apply.

---

## Test Pyramid

We follow the standard test pyramid. The distribution of tests should roughly mirror this shape:

```
         /\
        /  \
       / E2E\          ← Few: critical user journeys only
      /------\
     /Integr. \        ← Moderate: service boundaries, DB, external APIs
    /----------\
   /    Unit    \      ← Many: business logic, pure functions, edge cases
  /--------------\
```

**Do not invert the pyramid.** A test suite heavy in E2E and light in unit tests is slow, fragile, and expensive to maintain.

---

## Test Types

### Unit Tests

**What:** Test a single function, method, or class in isolation. All external dependencies (databases, HTTP clients, clocks) are replaced with doubles (mocks, stubs, fakes).

**Required for:**
- All business logic in the service layer.
- Utility functions and helpers.
- Input validation logic.
- Error handling paths.

**Not required for:**
- Pure data transfer objects (no logic).
- Framework boilerplate that has no custom logic.

**Standard:**
- Tests must be fast (< 100ms per test).
- Tests must be deterministic — no randomness, no time dependencies, no network calls.
- Use `describe` / `it` (or language equivalent) to express behavior, not implementation.

---

### Integration Tests

**What:** Test the interaction between a service and its real dependencies (database, cache, message bus, other services). Run against real infrastructure, typically via Docker Compose in CI.

**Required for:**
- All repository / data access layer methods.
- Message bus consumer and producer behavior.
- Any code path that involves a transaction or multi-step database operation.

**Standard:**
- Each test should set up and tear down its own data — no shared state between tests.
- Use test containers or Docker Compose for local and CI runs.
- Integration tests may be slower than unit tests but must complete the full suite in < 5 minutes in CI.

---

### End-to-End (E2E) Tests

**What:** Test complete user journeys through the full deployed stack.

**Required for:**
- Critical user-facing flows (authentication, core purchase/signup path, etc.).
- Smoke tests run after every production deployment.

**Standard:**
- Keep the E2E suite small and focused — only cover flows that cannot be adequately covered at a lower level.
- E2E tests must be stable. A flaky E2E test must be fixed or removed within one sprint.
- E2E tests run in the CI pipeline against the staging environment.

---

### Contract Tests

**What:** Verify that a service's API (producer) meets the expectations of its consumers, without running the whole stack.

**Recommended for:**
- Services with multiple consumers.
- Services where API changes could silently break downstream consumers.

**Tooling:** Consumer-driven contract testing (e.g., Pact) is preferred.

---

## Coverage Expectations

| Layer | Minimum Coverage | Notes |
|---|---|---|
| Service / Business Logic | **80%** line coverage | Core logic should be higher (90%+). Coverage alone is not a quality metric. |
| Repository / Data Access | All paths covered via integration tests | Focus on behavior, not line coverage numbers. |
| Handler / Router | All happy paths + key error cases | Covered by integration or E2E. |

**Coverage is a floor, not a goal.** Do not write tests purely to hit a number.

---

## What Must Be Tested (Per Change Type)

| Change Type | Required Tests |
|---|---|
| New feature | Unit tests for business logic + integration test for data layer + E2E for user-facing journeys |
| Bug fix | Unit test that reproduces the bug (regression test) before the fix |
| Refactor (no behavior change) | Existing tests must pass; no new tests required unless coverage drops below threshold |
| New API endpoint | Unit tests for validation/auth logic + integration test for the endpoint |
| Database migration | Integration test that runs the migration and verifies the resulting schema and data |
| New event consumer | Integration test that processes a test event and verifies the side effect |

---

## Test Naming Conventions

Use the pattern: `[Unit of work] — [state/scenario] — [expected outcome]`

Examples:
- `createOrder — when stock is insufficient — throws InsufficientStockError`
- `validateEmail — given a malformed address — returns validation error`
- `POST /users — when email is already registered — returns 409 Conflict`

---

## Test Data

- Unit tests: use inline fixtures or factory functions. No shared state.
- Integration tests: each test creates its own data and cleans up after itself.
- **Never use production data in tests** — see [`compliance/data-classification.md`](../compliance/data-classification.md).
- Use realistic but fake data (libraries like `faker` are acceptable).

---

## CI Requirements

- All tests must pass in CI before a PR can be merged.
- Tests must not be skipped in CI without a tracked issue and team lead approval.
- Flaky tests must be tracked in the issue tracker and fixed within one sprint.

---

## Related Documents

- [`standards/coding-standards.md`](./coding-standards.md)
- [`policies/code-review-policy.md`](../policies/code-review-policy.md)
- [`compliance/data-classification.md`](../compliance/data-classification.md)
