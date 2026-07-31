# Vendor Approval Policy

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering / Legal  
**Last Updated:** 2026-07-31

---

## Purpose

This policy defines how the organization evaluates and approves new vendors, third-party services, and external integrations. It exists to prevent shadow IT, protect sensitive data, and ensure new dependencies are reviewed against security, compliance, and operational constraints.

---

## When This Policy Applies

This policy applies whenever a team proposes to:
- Integrate a new third-party service or vendor.
- Send organizational or customer data to an external service.
- Replace an existing vendor or add a new SaaS tool to the stack.
- Use a new AI/ML API or other external inference service.

---

## Approval Process

### 1. Initial Request

The proposing team must create a short approval request that includes:
- The vendor/service name.
- The business or technical purpose.
- The data that would be shared with the vendor.
- The expected cost and licensing model.
- The proposed owner and support model.
- Any security or compliance concerns.

### 2. Security & Compliance Review

The request must be reviewed against:
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md)
- [`compliance/data-classification.md`](../compliance/data-classification.md)

The review must confirm:
- The vendor is suitable for the data class involved.
- Appropriate contractual protections (e.g., DPA) are in place where required.
- The vendor will not introduce unapproved security or compliance risk.

### 3. Technical Review

The technical review must confirm:
- The vendor is appropriate for the use case.
- The integration follows the relevant reference architecture.
- The vendor is listed in [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md) or is being added through a documented approval process.
- Operational impact is understood (monitoring, support, incident response, rollback).

### 4. Approval Decision

Approval requires sign-off from:
- The owning team lead.
- Security or compliance reviewer (if personal data, secrets, or regulated data are involved).
- Engineering leadership (for significant or cross-cutting vendors).

No integration may proceed until the approval decision is recorded.

---

## Prohibited Practices

The following are not permitted without approval:
- Sending production data to an unapproved third-party service.
- Adding a vendor that would create a new compliance obligation without legal review.
- Using a service that has no data retention / deletion story.
- Integrating a vendor without an owner and support plan.

---

## Enforcement

If a team integrates a vendor without approval, the integration must be stopped and reviewed. The incident may be escalated per [`runbooks/incident-response.md`](../runbooks/incident-response.md).

---

## Related Documents

- [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md)
- [`policies/data-handling-policy.md`](./data-handling-policy.md)
- [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md)
