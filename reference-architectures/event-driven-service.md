# Reference Architecture: Event-Driven Service

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Overview

This reference architecture defines the canonical pattern for an asynchronous, message-based service in this organization. Use it when a service primarily reacts to events from other services, or when operations do not require an immediate synchronous response. Deviations must be documented in an [ADR](../decision-logs/template.md).

---

## Component Diagram

```
  Upstream Services
  (producers)
       │
       │ publish domain events
       ▼
┌──────────────────────────────────────────────────────┐
│                  Event Bus (Kafka / SQS)              │
│  topic: orders.created   topic: payments.processed   │
└────────────────────────┬─────────────────────────────┘
                         │ consume
          ┌──────────────▼────────────────────┐
          │        Event-Driven Service         │
          │  ┌─────────────────────────────┐   │
          │  │  Consumer / Dispatcher       │   │
          │  │  (deserialize, route)        │   │
          │  └──────────────┬──────────────┘   │
          │  ┌──────────────▼──────────────┐   │
          │  │  Handler / Business Logic    │   │
          │  └──────────────┬──────────────┘   │
          │  ┌──────────────▼──────────────┐   │
          │  │  Repository / Data Access    │   │
          │  └──────────────┬──────────────┘   │
          └─────────────────┼─────────────────-┘
                   ┌────────┤
       ┌───────────▼──┐   ┌─▼───────────────────┐
       │  Primary DB   │   │  Outbox / Event Bus  │
       │  (PostgreSQL) │   │  (publish new events)│
       └───────────────┘   └──────────────────────┘
          │
       ┌──▼────────────────────────┐
       │  Optional: REST API        │
       │  (query/read interface)    │
       └────────────────────────────┘
```

---

## Components

### Event Bus (Kafka / SQS)
- The backbone for inter-service communication.
- Producers publish immutable, versioned domain events (e.g., `order.created.v1`).
- Topics are named `<domain>.<event>.<version>`: `payments.processed.v1`.
- The event bus provides at-least-once delivery; consumers must be idempotent.

### Consumer / Dispatcher
- Reads messages from the event bus (pull model).
- Deserializes the event envelope and routes to the appropriate handler by event type.
- Performs schema validation — reject and dead-letter malformed events rather than crashing.
- Handles consumer group coordination and offset management.

### Handler / Business Logic Layer
- Stateless; receives a deserialized event and an injected set of dependencies.
- All database writes use the **Transactional Outbox pattern** (see below) to guarantee event publication is atomic with the state change.
- Idempotency key (typically the event `id`) must be checked before processing to prevent duplicate side effects.

### Repository / Data Access Layer
- Same role as in the REST API reference architecture.
- Implements the outbox table write as part of the same transaction as the state mutation.

### Transactional Outbox
- A table in the primary database records events-to-publish in the same transaction as state changes.
- A separate relay process reads the outbox and publishes to the event bus, then marks records as sent.
- This eliminates the dual-write problem (state saved but event not published, or vice versa).

### Optional REST API
- Event-driven services may expose a read-only HTTP API to serve query traffic.
- Follow the [REST API Service reference architecture](./rest-api-service.md) for that interface.
- CQRS (Command Query Responsibility Segregation) is the recommended pattern: events mutate state; the API serves reads from a read-optimized projection.

---

## Data Flow: Processing an Incoming Event

```
Event Bus
  → Consumer (message received, deserialized, schema validated)
  → Dispatcher (routed to handler by event type)
  → Handler (idempotency check — already processed? skip)
  → Handler (business logic executed)
  → Repository (state mutation + outbox row — single transaction committed)
  → Outbox Relay (reads outbox, publishes new domain events to event bus)
  → Consumer (acknowledges / commits offset)
```

---

## Event Schema

All events must follow this envelope:

```json
{
  "id": "evt_01j9xk2p3qr5s6t7u8v9",
  "type": "order.created.v1",
  "source": "order-service",
  "timestamp": "2026-07-31T12:00:00Z",
  "data": {
    "order_id": "ord_123",
    "customer_id": "cust_456",
    "total_cents": 4999
  }
}
```

- `id` — unique, immutable. Used as the idempotency key by consumers.
- `type` — namespaced event type with version suffix.
- `source` — the service that produced the event.
- `timestamp` — UTC ISO 8601.
- `data` — the event payload. Schema is versioned and published to the schema registry.

---

## Cross-Cutting Concerns

### Idempotency
- Every handler must check whether the event `id` has already been processed.
- Store processed event IDs in a deduplication table (or use a unique constraint on the idempotency key).
- Processing the same event twice must have the same observable outcome as processing it once.

### Error Handling & Dead Letter Queue
- Transient errors (network, DB unavailable): retry with exponential back-off (max 5 retries).
- Permanent errors (invalid schema, unrecognized event type): route to the Dead Letter Queue (DLQ).
- DLQ messages must alert on-call and be reviewed within 4 hours.
- Never silently discard a failed message.

### Observability
- Log every event consumed: `event_id`, `event_type`, `source`, `consumer_group`, `processing_duration_ms`, `outcome` (success / retry / dlq).
- Emit a metric per event type: consumed count, error count, processing latency (p50/p95/p99).
- Expose `/healthz` and `/readyz` for liveness and readiness probes.
- Alert on consumer lag exceeding threshold (tune per topic SLA).

### Schema Evolution
- Use the schema registry for all event schemas (Avro or JSON Schema).
- Apply **backward-compatible** changes by default (add optional fields, do not remove or rename).
- Breaking changes require a new version suffix (`order.created.v2`) and a migration period where both versions are published.

---

## Technology Choices

| Concern | Canonical Choice | Notes |
|---|---|---|
| Event bus | Kafka (high throughput) or AWS SQS/SNS (simpler ops) | Document choice in ADR |
| Schema registry | Confluent Schema Registry (Kafka) or EventBridge schema registry (SQS) | Required |
| Language | TypeScript (Node.js) or Go | Per team stack |
| Database | PostgreSQL (with outbox table) | |
| Outbox relay | Debezium CDC or custom relay process | Debezium preferred for Kafka |
| Containerization | Docker | Required |
| Orchestration | Kubernetes | Standard deployment target |

---

## Known Trade-offs

| Decision | Trade-off |
|---|---|
| At-least-once delivery | Simpler infrastructure than exactly-once; requires consumer idempotency |
| Transactional outbox | Adds complexity (outbox table + relay) but eliminates dual-write race conditions |
| Schema registry | Adds operational dependency but prevents schema drift across services |
| Async processing | Higher throughput and decoupling, but harder to debug and trace end-to-end |

---

## Related Documents

- [REST API Service Reference Architecture](./rest-api-service.md)
- [Architecture Framework](../frameworks/architecture-framework.md)
- [Decision-Making Framework](../frameworks/decision-making-framework.md)
- [Security Standards](../standards/security-standards.md)
