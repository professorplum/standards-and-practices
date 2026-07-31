# Tech Radar: Data Stores

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Relational Databases

| Technology | Ring | Notes |
|---|---|---|
| **PostgreSQL** | ✅ Approved | Default relational database. Use for all new services requiring a relational store. |
| **SQLite** | ⚠️ Restricted | Permitted for local development, CLI tools, and embedded use cases only. Not permitted as a production service database. |
| **MySQL / MariaDB** | ⚠️ Restricted | Permitted only in legacy services. No new MySQL databases without ADR justification. |
| **Oracle DB** | 🚫 Banned | Licensing cost and vendor lock-in. Migrate existing uses to PostgreSQL. |
| **SQL Server** | 🚫 Banned | Licensing cost and Windows dependency. |

---

## Caching

| Technology | Ring | Notes |
|---|---|---|
| **Redis** | ✅ Approved | Standard caching layer. Use Redis Cluster for production deployments at scale. Define cache invalidation strategy before adding. |
| **Memcached** | 🚫 Banned | Redis is preferred; Memcached offers no advantages for our workloads. |
| **In-process / in-memory cache** | ⚠️ Restricted | Permitted only for immutable reference data. Must not be used for data that requires consistency across instances. |

---

## Message / Event Brokers

| Technology | Ring | Notes |
|---|---|---|
| **Apache Kafka** | ✅ Approved | Standard event bus. Use for domain events and high-throughput async communication. |
| **RabbitMQ** | 🔵 Trial | Permitted for task queues and point-to-point messaging where Kafka is overkill. Evaluate before committing. |
| **AWS SQS / SNS** | ⚠️ Restricted | Permitted in cloud-native deployments where Kafka introduces operational overhead. Requires ADR. |
| **Redis Pub/Sub** | ⚠️ Restricted | No persistence, no consumer groups. Permitted only for ephemeral notifications (e.g., WebSocket fan-out). Not for durable event streams. |

---

## Search

| Technology | Ring | Notes |
|---|---|---|
| **Elasticsearch / OpenSearch** | 🔵 Trial | Approved for full-text search workloads. Do not use as a primary data store; always have a source of truth in PostgreSQL. |
| **PostgreSQL full-text search** | ✅ Approved | Preferred for moderate search requirements. Avoids additional infrastructure. |

---

## Object / File Storage

| Technology | Ring | Notes |
|---|---|---|
| **S3-compatible object storage** | ✅ Approved | Standard for blob/file storage. All uploads must use pre-signed URLs; never expose storage credentials to clients. |

---

## Time-Series Databases

| Technology | Ring | Notes |
|---|---|---|
| **InfluxDB / TimescaleDB** | 🔵 Trial | Permitted for metrics and time-series workloads where PostgreSQL is insufficient. Requires ADR. |

---

## Schema & Migration Tools

| Technology | Ring | Notes |
|---|---|---|
| **golang-migrate** | ✅ Approved | Standard for Go services. |
| **Flyway** | ✅ Approved | Standard for JVM services. |
| **Alembic** | ✅ Approved | Standard for Python services. |
| **Liquibase** | ⚠️ Restricted | Permitted in existing projects. No new Liquibase projects; prefer Flyway. |
| **ORM auto-migrations** | 🚫 Banned | Never use ORM-generated auto-migrations in production. All schema changes must be explicit, versioned migration scripts. |

---

## Related Documents

- [`tech-radar/README.md`](./README.md)
- [`tech-radar/infrastructure-and-cloud.md`](./infrastructure-and-cloud.md)
- [`reference-architectures/rest-api-service.md`](../reference-architectures/rest-api-service.md)
