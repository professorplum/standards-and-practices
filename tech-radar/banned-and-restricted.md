# Tech Radar: Banned & Restricted Technologies

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Purpose

This is the first document an AI agent or engineer should check when evaluating a technology choice. Banned technologies must not be used — not in new code, not in workarounds, not in "temporary" solutions.

Restricted technologies have specific conditions under which they are permitted. If those conditions do not apply, treat the technology as banned.

---

## Banned Technologies

Technologies in this list must **not** be used in any new code. Existing usages must be tracked and scheduled for removal.

| Technology | Category | Reason | Migration Path |
|---|---|---|---|
| **Ruby** | Language | Small team expertise, slow runtime, high maintenance cost | TypeScript / Go |
| **PHP** | Language | Security history, inconsistent ecosystem | TypeScript / Go |
| **Oracle DB** | Database | Prohibitive licensing cost, vendor lock-in | PostgreSQL |
| **SQL Server** | Database | Licensing cost, Windows dependency | PostgreSQL |
| **Memcached** | Cache | Redis is superior in all relevant dimensions | Redis |
| **ORM auto-migrations** | Database tooling | Silent schema changes in production; data loss risk | Explicit versioned migration scripts |
| **Jenkins** | CI/CD | Replaced by GitHub Actions; high maintenance overhead | GitHub Actions |
| **Nomad** | Orchestration | Kubernetes is the standard | Kubernetes |
| **Hardcoded secrets** | Security | Absolute security violation | Secrets manager |
| **`.env` files in production** | Security | Secrets must not be in the filesystem | Secrets manager |
| **SHA-1 / MD5 for passwords** | Security | Cryptographically broken | bcrypt / argon2 / scrypt |
| **Self-signed TLS in staging/production** | Security | No real certificate validation | Certificate Authority–issued certs |
| **HTTP (non-TLS) in production** | Security | Data in transit unprotected | HTTPS with TLS 1.2+ |
| **Manual ClickOps in staging/production** | Infrastructure | Non-reproducible, audit-gap, drift risk | Terraform / IaC |
| **New Relic** | Observability | Consolidating on Prometheus + Grafana + OTel | Prometheus / Grafana / OTel |
| **HAProxy (new deployments)** | Networking | Legacy; Nginx / Traefik is preferred | Nginx |
| **yarn classic (v1) (new projects)** | Package manager | Outdated; use npm or pnpm | npm / pnpm |

---

## Restricted Technologies

Technologies in this list are allowed **only under the stated conditions**. Using them outside those conditions requires an ADR and team lead approval.

| Technology | Condition | Notes |
|---|---|---|
| **JavaScript (plain)** | Frontend only, where TypeScript is unsupported | All new frontend must use TypeScript |
| **Bash / Shell scripts** | CI scripts and simple automation ≤ ~50 lines | No business logic |
| **Java** | Existing services only | No new Java services without ADR |
| **SQLite** | Local dev, CLI tools, embedded contexts | Not a production service database |
| **MySQL / MariaDB** | Legacy services only | No new MySQL databases without ADR |
| **In-process cache** | Immutable reference data only | Must not be used for data requiring cross-instance consistency |
| **Redis Pub/Sub** | Ephemeral notifications only (e.g., WebSocket fan-out) | No persistence; not for durable event streams |
| **ECS / Fargate** | Cloud-native migrations where Kubernetes unavailable | Requires ADR |
| **AWS CloudFormation** | AWS-specific contexts where Terraform insufficient | |
| **OpenAI / Azure OpenAI APIs** | Internal tooling + features with completed privacy review | Must not send PII without a DPA |
| **Multi-cloud** | Specific DR or compliance scenarios only | Requires ADR; high operational cost |
| **Python for API services** | Data pipelines, ML workloads, scripting | Not for primary API services without ADR |

---

## How to Challenge a Ban or Restriction

A ban is not permanent if circumstances change. To challenge:
1. Write an ADR explaining: why the ban should be lifted, what changed, what safeguards apply.
2. Get tech lead review and VP Engineering approval.
3. Update this document upon approval with the new status and ADR reference.

---

## Related Documents

- [`tech-radar/README.md`](./README.md)
- [`tech-radar/languages-and-runtimes.md`](./languages-and-runtimes.md)
- [`tech-radar/data-stores.md`](./data-stores.md)
- [`tech-radar/infrastructure-and-cloud.md`](./infrastructure-and-cloud.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`frameworks/decision-making-framework.md`](../frameworks/decision-making-framework.md)
