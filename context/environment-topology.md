# Environment Topology

**Version:** 1.0  
**Status:** Active  
**Owner:** Platform / DevOps  
**Last Updated:** 2026-07-31

---

## Overview

This document describes the environments in which this organization's software runs, their purposes, access controls, and naming conventions. All deployment proposals must target one of these environments and follow the conventions defined here.

---

## Environments

| Environment | Short Name | Purpose | Access | Mirrors Production? |
|---|---|---|---|---|
| Local Development | `local` | Developer workstation; no shared infrastructure | Developer only | No |
| Development | `dev` | Shared integration environment; latest builds deployed automatically | Engineering | No |
| Staging | `staging` | Pre-production; mirrors production config; used for final validation | Engineering + QA | Yes |
| Production | `prod` | Live environment; serves real users | Restricted (see below) | — |

---

## Environment Details

### Local Development
- Each developer runs services locally using Docker Compose or equivalent.
- Local config is managed via `.env` files (see [`standards/coding-standards.md`](../standards/coding-standards.md) for conventions).
- Local databases are ephemeral; seed scripts are maintained per service.
- No production data may be used locally — use anonymized or synthetic datasets only.

### Development (`dev`)
- Deployed automatically on merge to `main`.
- Separate database instances from staging and production.
- External integrations use sandbox/test credentials.
- Not suitable for performance testing — infrastructure is under-provisioned relative to production.
- URL pattern: `https://<service>.dev.<domain>`

### Staging (`staging`)
- Deployed manually or via the release pipeline; see [`runbooks/deployment-runbook.md`](../runbooks/deployment-runbook.md).
- Infrastructure configuration mirrors production (instance sizes, replica counts, CDN, etc.).
- External integrations use sandbox/test credentials — **never production credentials in staging**.
- Required validation gate before any production deployment.
- URL pattern: `https://<service>.staging.<domain>`

### Production (`prod`)
- Deployed via the approved pipeline only — no ad-hoc deployments.
- Access is restricted: see production access policy in [`policies/deployment-policy.md`](../policies/deployment-policy.md).
- All changes must pass staging validation.
- URL pattern: `https://<service>.<domain>`

---

## Naming Conventions

### Service Hostnames
```
<service-name>.<env>.<domain>        # dev and staging
<service-name>.<domain>              # production
```

### Infrastructure Resource Naming
```
<org>-<env>-<service>-<resource>
# Example: acme-prod-orders-db, acme-staging-users-cache
```

### Configuration & Secrets
- Environment-specific config is injected via environment variables.
- Secret naming follows: `<SERVICE>_<CATEGORY>_<NAME>` in uppercase.
  - Example: `ORDERS_DB_PASSWORD`, `AUTH_JWT_PRIVATE_KEY`
- Shared secrets (used by multiple services) are prefixed with `SHARED_`.

---

## Access Controls

| Environment | Who Can Deploy? | Who Has DB Access? | Who Can SSH/Exec? |
|---|---|---|---|
| `local` | Developer (self) | Developer (self) | Developer (self) |
| `dev` | CI/CD pipeline, any engineer | Any engineer (read/write) | Any engineer |
| `staging` | CI/CD pipeline, release manager | Senior engineers (read), DBA (write) | Senior engineers |
| `prod` | CI/CD pipeline, VP Engineering (break-glass) | DBA only | SRE on-call (break-glass) |

Break-glass access to production requires logging in the incident log and post-incident review.

---

## Infrastructure Platform

- **Cloud Provider:** See [`tech-radar/infrastructure-and-cloud.md`](../tech-radar/infrastructure-and-cloud.md) for approved cloud services.
- **Container Orchestration:** Kubernetes (all environments except local).
- **Service Mesh / Networking:** Services communicate via internal DNS within a cluster; external traffic enters via the API gateway.
- **Secrets:** All environments above `local` use the organization's secrets manager — environment variables for secrets are not permitted outside local development.

---

## Related Documents

- [`context/service-catalog.md`](./service-catalog.md)
- [`policies/deployment-policy.md`](../policies/deployment-policy.md)
- [`runbooks/deployment-runbook.md`](../runbooks/deployment-runbook.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
