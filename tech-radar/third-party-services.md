# Tech Radar: Third-Party Services & Vendors

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Overview

This document lists approved third-party SaaS products, APIs, and external vendors. Before integrating a new external service, check this list. If a service is not listed, follow the vendor approval process in [`policies/vendor-approval-policy.md`](../policies/vendor-approval-policy.md) **before** integrating.

**AI agents:** Do not propose or implement integration with an unlisted third-party service. Flag it as requiring vendor approval.

---

## Communication & Notifications

| Service | Purpose | Ring | Notes |
|---|---|---|---|
| **[Approved Email Provider]** | Transactional email | ✅ Approved | Replace placeholder with the approved provider once vendor approval is completed. Used by `notification-service`. |
| **[Approved SMS Provider]** | Transactional SMS | ✅ Approved | Replace placeholder with the approved provider once vendor approval is completed. Used by `notification-service`. |
| **Slack** | Internal team communication | ✅ Approved | Webhook integrations permitted for alerts and notifications. |

---

## Identity & Authentication

| Service | Purpose | Ring | Notes |
|---|---|---|---|
| **[OAuth Provider]** | Social login (Google, GitHub, etc.) | ✅ Approved | Replace placeholder with approved provider(s). Handled via `auth-service`. |

---

## Monitoring & Alerting

| Service | Purpose | Ring | Notes |
|---|---|---|---|
| **PagerDuty / OpsGenie** | On-call alerting | ✅ Approved | Replace with the specific tool in use. |
| **Datadog** | Managed observability | 🔵 Trial | See [`tech-radar/infrastructure-and-cloud.md`](./infrastructure-and-cloud.md). Cost review required for production. |

---

## Developer Tooling

| Service | Purpose | Ring | Notes |
|---|---|---|---|
| **GitHub** | Source control, CI/CD, project tracking | ✅ Approved | Primary platform for all engineering work. |
| **GitHub Copilot** | AI coding assistance | ✅ Approved | Do not submit generated code without review. Follow standard code review policy. |

---

## Payments & Financial

| Service | Purpose | Ring | Notes |
|---|---|---|---|
| _(Add when applicable)_ | | | Payment services require additional PCI-DSS compliance review; see [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md). |

---

## AI / ML Platforms

| Service | Purpose | Ring | Notes |
|---|---|---|---|
| **OpenAI API / Azure OpenAI** | LLM inference | ⚠️ Restricted | Permitted for internal tooling and features that have completed a privacy review. Must not send PII or confidential data to external APIs without explicit data processing agreements. Requires team lead approval. |
| **Unlisted AI/ML APIs** | Any | 🚫 Not Approved | Treat as unapproved until reviewed. See vendor approval process. |

---

## Banned Vendors

| Vendor / Service | Reason |
|---|---|
| _(Add as bans are established)_ | See [`banned-and-restricted.md`](./banned-and-restricted.md) |

---

## Adding a New Third-Party Service

1. Do NOT begin integration before approval.
2. Submit a vendor approval request per [`policies/vendor-approval-policy.md`](../policies/vendor-approval-policy.md).
3. Key questions for approval:
   - What data will be sent to this service?
   - Does the service have a DPA (Data Processing Agreement) if it handles personal data?
   - What is the fallback if the service goes down?
   - What are the licensing and cost implications?
4. Upon approval, add the service to this document.

---

## Related Documents

- [`policies/vendor-approval-policy.md`](../policies/vendor-approval-policy.md)
- [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md)
- [`tech-radar/banned-and-restricted.md`](./banned-and-restricted.md)
