# Team & Ownership

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Purpose

This document identifies who owns what. Use it to:
- Find the right team to loop in when a change crosses boundaries.
- Know who to escalate to for architectural or policy decisions.
- Identify on-call contacts for operational issues.

**AI agents:** When a task touches code or infrastructure owned by a different team, or when a decision requires human approval, identify the owner here before proceeding.

---

## Teams

> **Note:** This is a template. Replace with your actual team structure and contacts.

---

### Platform Team

**Mission:** Build and operate the shared infrastructure, developer tooling, and core services that all other teams depend on.

| Role | Responsibility |
|---|---|
| Team Lead | Technical decisions for platform services; ADR approvals |
| On-Call Engineer | Incident response for shared infrastructure |

**Owns:**
- `auth-service`
- `user-service`
- `notification-service`
- `api-gateway`
- CI/CD pipelines
- Kubernetes cluster configuration
- Secrets management infrastructure

**Contact:** `#platform` (Slack / Teams channel)  
**Escalation:** Team Lead → VP Engineering

---

### Product Engineering Team(s)

**Mission:** Build and maintain the product features that create value for customers.

> Add one block per product team as the organization grows.

**Contact:** `#product-eng`  
**Escalation:** Team Lead → VP Engineering

---

## Ownership Matrix

| Domain / System | Owning Team | Primary Contact | Notes |
|---|---|---|---|
| `auth-service` | Platform | Team Lead | Security-sensitive; loop in Platform for any changes |
| `user-service` | Platform | Team Lead | PII-heavy; loop in Platform + compliance reviewer |
| `notification-service` | Platform | Team Lead | |
| `api-gateway` | Platform | Team Lead | Changes affect all services |
| CI/CD infrastructure | Platform | Team Lead | |
| Kubernetes / orchestration | Platform | Team Lead | Infra changes require Platform approval |
| Security standards | Security / Engineering | Team Lead | Changes require VP Engineering sign-off |
| Compliance policy | Legal / Engineering | Team Lead | Changes require Legal review |
| Architecture decisions | Engineering Leadership | Tech Lead / Architect | ADR required |

---

## Escalation Paths

```
Operational Issue (service down, data loss, security incident)
  → On-Call Engineer
  → Team Lead
  → VP Engineering
  → CTO (if customer-impacting and unresolved within SLA)

Architecture / Design Decision
  → Feature Owner (proposes ADR)
  → Tech Lead / Architect (reviews)
  → VP Engineering (approves for cross-cutting changes)

Security Incident
  → Security Team / On-Call Engineer (immediately)
  → Do NOT attempt self-remediation without security team involvement
  → See runbooks/incident-response.md

Policy / Compliance Question
  → Team Lead
  → Legal / Compliance
```

---

## Rotation & On-Call

On-call schedules are managed in [PagerDuty / OpsGenie / your tool]. The current on-call engineer is always reachable via `#incidents`.

- On-call rotation: Weekly, rotating through senior engineers on the Platform team.
- Escalation after 15 minutes without acknowledgment: Team Lead is paged.
- After-hours escalation: VP Engineering for P1 incidents.

---

## Related Documents

- [`context/service-catalog.md`](./service-catalog.md)
- [`runbooks/incident-response.md`](../runbooks/incident-response.md)
- [`runbooks/on-call-guide.md`](../runbooks/on-call-guide.md)
- [`policies/deployment-policy.md`](../policies/deployment-policy.md)
