# Observability Standards

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering / Platform  
**Last Updated:** 2026-07-31

---

## Overview

Observability is the ability to understand what a system is doing from its external outputs. A system that cannot be observed cannot be reliably operated. These standards define the minimum instrumentation required for all production services.

The three pillars: **Logs**, **Metrics**, **Traces**.

---

## Logs

### Format

All logs must be structured JSON. No plain-text log lines in production.

Required fields for every log entry:

```json
{
  "timestamp": "2026-07-31T13:00:00.000Z",
  "level": "info",
  "service": "orders-service",
  "version": "1.4.2",
  "message": "Order created successfully",
  "request_id": "req_abc123"
}
```

Additional fields for HTTP requests:

```json
{
  "method": "POST",
  "path": "/orders",
  "status_code": 201,
  "duration_ms": 42,
  "user_id": "usr_xyz"
}
```

### Log Levels

| Level | When to Use |
|---|---|
| `trace` | High-frequency diagnostic data. Never enabled in production by default. |
| `debug` | Developer-oriented diagnostic context. Disabled in production by default. |
| `info` | Normal operational events (request handled, job processed, service started). |
| `warn` | Something unexpected happened but the request succeeded; may need investigation. |
| `error` | A request or operation failed due to an error. Includes stack traces. |
| `fatal` | The service is about to crash. Immediately before process exit. |

### What to Log

**Always log:**
- Service startup and shutdown.
- All authentication events (success and failure).
- All authorization failures.
- Significant state mutations (create, update, delete) — include actor identity and entity ID.
- All `error` and `fatal` events with stack traces.
- Slow operations exceeding defined thresholds.

**Never log:**
- Passwords, tokens, API keys, or any secrets.
- PII (email, phone, address, SSN) — see [`compliance/data-classification.md`](../compliance/data-classification.md).
- Full request/response bodies.
- Health check requests (`/healthz`, `/readyz`).

### Log Retention

- Minimum 90 days in the log aggregation system.
- Access logs for production systems: 1 year (SOC 2 / compliance requirement).
- Logs must be searchable and filterable by `request_id`, `user_id`, `service`, and time range.

---

## Metrics

### Required Endpoints

Every service must expose a `/metrics` endpoint in **Prometheus text format**.

### Required Metrics (per service)

| Metric | Type | Description |
|---|---|---|
| `http_requests_total` | Counter | Total HTTP requests, labeled by `method`, `path`, `status_code` |
| `http_request_duration_seconds` | Histogram | Request latency, labeled by `method`, `path` |
| `http_errors_total` | Counter | Total errors (4xx/5xx), labeled by `method`, `path`, `status_code` |
| `db_query_duration_seconds` | Histogram | Database query latency, labeled by `operation` |
| `process_cpu_seconds_total` | Counter | Standard process metrics |
| `process_resident_memory_bytes` | Gauge | Standard process metrics |

RED metrics (Rate, Errors, Duration) per endpoint are the minimum. Add domain-specific business metrics as needed.

### Metric Naming

Follow Prometheus naming conventions:
- Use `snake_case`.
- Suffix with the unit: `_seconds`, `_bytes`, `_total`, `_ratio`.
- Prefix with the service name for custom domain metrics: `orders_created_total`.

---

## Traces

All services must instrument distributed tracing using the **OpenTelemetry SDK**.

**Required:**
- Trace context propagation via `traceparent` / `tracestate` headers (W3C Trace Context).
- Every inbound HTTP request creates a root span.
- Every outbound HTTP call creates a child span.
- Every database query creates a child span.
- Every message bus publish/consume creates a span.

The `request_id` in logs must match the trace ID in the distributed tracing system to enable log-trace correlation.

---

## Health Endpoints

Every service must expose:

| Endpoint | Purpose | Returns |
|---|---|---|
| `GET /healthz` | Liveness — is the process alive? | `200 OK` if the process is running; no dependency checks |
| `GET /readyz` | Readiness — is the service ready to handle traffic? | `200 OK` if all critical dependencies (DB, cache) are reachable; `503` otherwise |

Kubernetes probes must be configured to use these endpoints. See the [REST API reference architecture](../reference-architectures/rest-api-service.md) for probe configuration.

---

## SLOs — Service Level Objectives

Every production service must define SLOs. At minimum:

| SLO | Default Target | Notes |
|---|---|---|
| Availability | 99.9% (3 nines) per rolling 30 days | Measured by `1 - (error_rate)` |
| p99 Latency | < 500ms | For synchronous API endpoints under normal load |
| p50 Latency | < 100ms | |

Define tighter SLOs for critical user-facing paths. Document SLOs in the service's README.

SLO breaches must trigger alerts routed to on-call via the alerting stack (Prometheus Alertmanager → PagerDuty / OpsGenie).

---

## Alerting Philosophy

- Alert on **symptoms** (user-visible impact), not **causes** (CPU high).
- Every alert must be actionable. If an alert fires and there is no action to take, remove or suppress it.
- Every alert must have a runbook link.
- P1 alerts page immediately. P2 alerts page within 5 minutes. P3 creates a ticket.
- Review alert noise quarterly — silence alerts that fire without leading to action.

---

## Related Documents

- [`standards/security-standards.md`](./security-standards.md)
- [`reference-architectures/rest-api-service.md`](../reference-architectures/rest-api-service.md)
- [`tech-radar/infrastructure-and-cloud.md`](../tech-radar/infrastructure-and-cloud.md)
- [`runbooks/incident-response.md`](../runbooks/incident-response.md)
- [`compliance/data-classification.md`](../compliance/data-classification.md)
