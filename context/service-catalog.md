# Service Catalog

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Overview

This catalog lists all services and systems owned or operated by this organization. It is the authoritative reference for:
- Knowing what already exists before building something new.
- Finding the owner of a service when changes cross team boundaries.
- Understanding how services communicate.

**AI agents:** Check this catalog before proposing a new service or referencing an external dependency.

---

## How to Read This Catalog

Each entry includes:
- **Purpose** — what the service does
- **Owner** — team responsible (see [`team-and-ownership.md`](./team-and-ownership.md))
- **Tech stack** — primary language and data store
- **Exposes** — APIs, events, or data it publishes
- **Depends on** — services or systems it consumes
- **Repo** — link to the source repository
- **Status** — Active / Deprecated / Planned

---

## Services

> **Note:** This catalog is a template. Replace placeholder entries with your actual services. Each entry should be kept current — update it when a service is added, deprecated, or significantly changed.

---

### `auth-service`

| Field | Value |
|---|---|
| **Purpose** | Issues and validates JWT tokens for user authentication; manages OAuth 2.0 flows |
| **Owner** | Platform Team |
| **Tech Stack** | TypeScript / Node.js, PostgreSQL |
| **Exposes** | REST API (`/auth/token`, `/auth/refresh`, `/auth/revoke`); publishes `user.login` and `user.logout` events |
| **Depends On** | `user-service` (user identity lookup), secrets manager (signing keys) |
| **Repo** | `<org>/auth-service` |
| **Status** | Active |

---

### `user-service`

| Field | Value |
|---|---|
| **Purpose** | Manages user profiles, preferences, and account lifecycle |
| **Owner** | Platform Team |
| **Tech Stack** | TypeScript / Node.js, PostgreSQL |
| **Exposes** | REST API (`/users`); publishes `user.created`, `user.updated`, `user.deleted` events |
| **Depends On** | `auth-service` (identity verification), `notification-service` (welcome/lifecycle emails) |
| **Repo** | `<org>/user-service` |
| **Status** | Active |

---

### `notification-service`

| Field | Value |
|---|---|
| **Purpose** | Sends transactional emails, SMS, and push notifications |
| **Owner** | Platform Team |
| **Tech Stack** | TypeScript / Node.js, PostgreSQL (audit log) |
| **Exposes** | REST API (`/notifications/send`); subscribes to domain events from other services |
| **Depends On** | Email provider (see [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md)), SMS provider |
| **Repo** | `<org>/notification-service` |
| **Status** | Active |

---

### `api-gateway`

| Field | Value |
|---|---|
| **Purpose** | Single ingress point for all external traffic; handles TLS termination, rate limiting, JWT validation, and routing |
| **Owner** | Platform Team |
| **Tech Stack** | Managed gateway (see tech-radar for approved product) |
| **Exposes** | Public HTTPS endpoints for all services |
| **Depends On** | `auth-service` (token validation), all downstream services |
| **Repo** | `<org>/infrastructure` (configuration) |
| **Status** | Active |

---

## Event Bus Topics

> All events follow the naming convention `<domain>.<event>` (e.g., `user.created`).

| Topic | Publisher | Consumers | Schema Location |
|---|---|---|---|
| `user.login` | `auth-service` | `notification-service`, audit log | `<org>/auth-service/events/` |
| `user.created` | `user-service` | `notification-service` | `<org>/user-service/events/` |
| `user.deleted` | `user-service` | All services holding user data | `<org>/user-service/events/` |

---

## Retired / Deprecated Services

| Service | Replaced By | Sunset Date |
|---|---|---|
| _(none yet)_ | — | — |

---

## Adding a New Service

Before adding a new service to production:
1. Check this catalog — does something similar already exist?
2. If creating a net-new service, follow the appropriate [reference architecture](../reference-architectures/).
3. Get team lead approval via the process in [`frameworks/decision-making-framework.md`](../frameworks/decision-making-framework.md).
4. Add the service to this catalog as part of the work (not after).

---

## Related Documents

- [`context/team-and-ownership.md`](./team-and-ownership.md)
- [`reference-architectures/`](../reference-architectures/)
- [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md)
