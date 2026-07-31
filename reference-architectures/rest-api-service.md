# Reference Architecture: REST API Service

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Overview

This reference architecture defines the canonical pattern for a synchronous HTTP/REST backend service in this organization. Use it as your starting point when building a new API service. Deviations must be documented in an [ADR](../decision-logs/template.md).

---

## Component Diagram

```
                        ┌────────────────────────────────────────┐
                        │            API Gateway / LB             │
                        │   (TLS termination, rate limiting,      │
                        │    auth token validation)               │
                        └───────────────┬────────────────────────┘
                                        │ HTTPS
                        ┌───────────────▼────────────────────────┐
                        │           REST API Service              │
                        │  ┌─────────────────────────────────┐   │
                        │  │  Router / Handler Layer          │   │
                        │  │  (input validation, auth checks) │   │
                        │  └──────────────┬──────────────────┘   │
                        │  ┌──────────────▼──────────────────┐   │
                        │  │  Service / Business Logic Layer  │   │
                        │  └──────────────┬──────────────────┘   │
                        │  ┌──────────────▼──────────────────┐   │
                        │  │  Repository / Data Access Layer  │   │
                        │  └──────────────┬──────────────────┘   │
                        └─────────────────┼──────────────────────┘
                              ┌───────────┤
              ┌───────────────▼──┐    ┌───▼───────────────┐
              │  Primary DB       │    │  Cache (optional)  │
              │  (PostgreSQL)     │    │  (Redis)           │
              └───────────────────┘    └────────────────────┘
                        │
              ┌─────────▼─────────────┐
              │  Async Event Bus       │
              │  (publish domain       │
              │   events on mutation)  │
              └────────────────────────┘
```

---

## Components

### API Gateway / Load Balancer
- Terminates TLS.
- Enforces rate limiting and DDoS protection.
- Validates and forwards auth tokens (JWT verification); does **not** own auth logic.
- Routes requests to the appropriate service.

### Router / Handler Layer
- Maps HTTP verbs and paths to handlers.
- Performs request deserialization and input validation — reject invalid input before it reaches business logic.
- Performs authorization checks (the caller has permission for this specific resource).
- Returns structured error responses with consistent shape.

### Service / Business Logic Layer
- Contains all domain logic.
- No direct database or HTTP calls — all I/O goes through injected dependencies (repositories, external clients).
- Stateless: no in-memory state that would break horizontal scaling.
- Orchestrates cross-cutting operations (e.g., send event after successful mutation).

### Repository / Data Access Layer
- Encapsulates all database queries.
- Provides typed, domain-meaningful methods (`FindUserByID`, `SaveOrder`) — no raw query strings in the service layer.
- Handles connection pooling and query timeout configuration.

### Primary Database (PostgreSQL)
- The service owns its schema. No other service queries this database directly.
- Schema changes are managed with versioned migrations (e.g., Flyway, golang-migrate, Alembic).
- Read replicas may be added for read-heavy workloads; the repository layer must route reads/writes explicitly.

### Cache (Redis — optional)
- Add only when profiling shows a specific performance need.
- Cache invalidation strategy must be defined before adding caching.
- Never cache data that has strict consistency requirements.

### Async Event Bus
- Publish domain events (e.g., `order.created`) after successful state mutations.
- Events are for notification, not commands — consumers must not assume ordering.
- At-least-once delivery; consumers must be idempotent.

---

## Data Flow: Successful Request

```
Client
  → API Gateway (auth token validated, rate check passed)
  → Router (deserialized, input validated, authorization checked)
  → Service (business logic executed)
  → Repository (database write)
  → Service (domain event published to event bus)
  → Router (response serialized)
  → Client (200 / 201 with response body)
```

---

## Cross-Cutting Concerns

### Authentication & Authorization
- Authentication is handled at the gateway; services receive a verified identity context.
- Authorization (can this identity perform this action on this resource?) is enforced in the handler layer.
- Follow [Security Standards — Authentication & Authorization](../standards/security-standards.md#authentication--authorization).

### Error Handling
- Use a consistent error response shape across all endpoints:
  ```json
  {
    "error": {
      "code": "RESOURCE_NOT_FOUND",
      "message": "User with id 42 was not found.",
      "request_id": "req_abc123"
    }
  }
  ```
- Map domain errors to HTTP status codes in the handler layer — do not leak internal error types.
- Log errors with the `request_id` for traceability.

### Observability
- Emit structured JSON logs including: `timestamp`, `level`, `request_id`, `user_id` (if available), `method`, `path`, `status_code`, `duration_ms`.
- Expose a `/healthz` (liveness) and `/readyz` (readiness) endpoint.
- Expose RED metrics (Rate, Errors, Duration) per endpoint via `/metrics` (Prometheus format).

### Configuration
- All environment-specific configuration (DB connection string, feature flags) via environment variables.
- Secrets via the organization secrets manager — not environment variables in production.
- Application must fail fast on startup if required configuration is missing.

---

## Technology Choices

| Concern | Canonical Choice | Notes |
|---|---|---|
| Language | TypeScript (Node.js) or Go | Per team stack; document in ADR if choosing another |
| HTTP framework | Express / Fastify (TS) or `net/http` (Go) | Avoid heavy opinionated MVC frameworks |
| Database | PostgreSQL | Use a different store only with ADR justification |
| Migrations | `golang-migrate` / Flyway / Alembic | Consistent with primary language |
| Cache | Redis | Only when needed |
| Auth tokens | JWT (RS256) | Validated at gateway |
| Containerization | Docker | Required for all services |
| Orchestration | Kubernetes | Standard deployment target |

---

## Known Trade-offs

| Decision | Trade-off |
|---|---|
| Layered architecture | Some overhead vs. a flat structure, but greatly improves testability and maintainability |
| Gateway-level auth validation | Reduces boilerplate per service, but requires secure inter-service trust if bypassing the gateway |
| At-least-once event delivery | Requires consumer idempotency; simpler than exactly-once infrastructure |
| PostgreSQL as default | May not be optimal for all access patterns (e.g., time-series, full-text search) — document exceptions |

---

## Related Documents

- [Event-Driven Service Reference Architecture](./event-driven-service.md)
- [Architecture Framework](../frameworks/architecture-framework.md)
- [Security Standards](../standards/security-standards.md)
- [Coding Standards](../standards/coding-standards.md)
- [POC Examples](../poc-examples/)
