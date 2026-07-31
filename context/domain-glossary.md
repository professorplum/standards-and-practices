# Domain Glossary

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Purpose

This glossary defines canonical terms used across this organization's systems and communications. Using consistent terminology prevents misunderstandings between humans and AI agents, avoids naming collisions in code, and ensures that documentation, PRs, and ADRs describe the same concepts the same way.

**Rule:** When a term appears in this glossary, use it as defined here — in code, documentation, APIs, database schemas, and conversation. If a term is used differently in an existing system, note the divergence and work toward alignment.

---

## How to Add Terms

Add new terms in alphabetical order. Include:
- **Definition** — what it means in this organization's domain
- **Used in** — which services or systems use this term
- **Aliases** — other names the same concept goes by (and which are preferred)
- **Do not confuse with** — terms that sound similar but mean something different

---

## Terms

---

### Account
The top-level entity representing an organization or individual who has contracted for service. An account may contain many users.

- **Used in:** `user-service`, `auth-service`, billing systems
- **Aliases:** Organization (deprecated — use "Account")
- **Do not confuse with:** User (a person within an account)

---

### Actor
Any entity (human user, service account, or AI agent) that performs an authenticated action in the system. Used in audit logs to identify who did what.

- **Used in:** All services with audit logging
- **Aliases:** Principal, Subject
- **Do not confuse with:** User (an Actor that is a human)

---

### Domain Event
A fact that something significant happened in the system, expressed in past tense (e.g., `order.created`, `user.deleted`). Domain events are published to the event bus and consumed by interested services. They are notifications, not commands.

- **Used in:** All services using the event bus
- **Aliases:** Event (acceptable shorthand in code)
- **Do not confuse with:** Command (tells a system to do something; a domain event records that something already happened)

---

### Environment
A named deployment context (local, dev, staging, prod). See [`context/environment-topology.md`](./environment-topology.md) for the full definition of each environment.

- **Do not confuse with:** Tenant, Account (these are data concepts, not infrastructure concepts)

---

### Identity
The verified representation of an Actor within the system, typically a JWT claims object containing subject (user ID), roles, and scopes. Identity is established by the `auth-service` and forwarded by the API gateway.

- **Used in:** `auth-service`, `api-gateway`, all services performing authorization
- **Aliases:** Auth context, Principal (acceptable in security contexts)

---

### PII (Personally Identifiable Information)
Any data that could be used to identify a specific individual. Includes but is not limited to: name, email address, phone number, physical address, government ID numbers, IP address (context-dependent), device identifiers.

- **Used in:** Compliance, data classification
- **See:** [`compliance/data-classification.md`](../compliance/data-classification.md)

---

### Service
An independently deployable unit of software that owns a specific domain and exposes a defined API or event stream. Services own their data and do not share databases. See the [service catalog](./service-catalog.md) for all current services.

- **Do not confuse with:** Module or library (compiled into another service; not independently deployable)

---

### Tenant
> **Note:** Add this term if the system is multi-tenant. Define the isolation model (schema-per-tenant, row-level security, separate deployments) here.

---

### User
A human individual who authenticates into the system and performs actions on behalf of themselves or an Account. Users have roles that determine their permissions.

- **Used in:** `user-service`, `auth-service`
- **Do not confuse with:** Account (the parent entity), Actor (the broader concept that includes non-human principals)

---

## Deprecated Terms

These terms have been superseded. Do not use them in new code or documentation.

| Deprecated Term | Use Instead | Notes |
|---|---|---|
| Organization | Account | Renamed for clarity |
| Principal | Actor | Actor is preferred in this org's context |

---

## Related Documents

- [`context/service-catalog.md`](./service-catalog.md)
- [`compliance/data-classification.md`](../compliance/data-classification.md)
- [`standards/documentation-standards.md`](../standards/documentation-standards.md)
