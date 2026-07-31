# Applicable Regulations

**Version:** 1.0  
**Status:** Active  
**Owner:** Legal / Engineering  
**Last Updated:** 2026-07-31

---

## Overview

This document identifies the regulatory frameworks that apply to this organization's systems and translates each into concrete engineering obligations. It is not a substitute for legal advice — consult Legal for specific compliance questions.

**Engineering obligation:** When building or modifying a system that touches personal data, data retention, or user rights, verify the relevant requirements in this document and in [`data-classification.md`](./data-classification.md).

---

## Applicable Frameworks

> **Note:** This is a template. Mark each framework as "Applies" or "Does not apply" based on your actual situation, and fill in the specifics. Consult your legal or compliance team to confirm applicability.

---

### GDPR — General Data Protection Regulation

**Applies to:** Systems that process personal data of individuals in the European Union or European Economic Area, regardless of where the organization is based.

**Status:** ☐ Applies / ☐ Does not apply / ☐ Under review

#### Engineering Obligations

| Requirement | What Engineering Must Do |
|---|---|
| **Lawful basis for processing** | Every collection of personal data must have a documented lawful basis (consent, contract, legitimate interest, etc.). Do not collect data without a basis. |
| **Data minimization** | Collect only the data necessary for the stated purpose. No speculative collection "in case it's useful later." |
| **Right to access** | Users can request a copy of all personal data held about them. Systems must support data export. |
| **Right to erasure ("right to be forgotten")** | Users can request deletion of their personal data. Systems must support full deletion (including backups within retention policy timelines). |
| **Data portability** | Users can request their data in a machine-readable format. |
| **Breach notification** | Suspected breaches involving personal data must be reported to the Data Protection Officer within **24 hours** of discovery (to meet the 72-hour regulatory window). See [`runbooks/incident-response.md`](../runbooks/incident-response.md). |
| **Data Processing Agreements (DPAs)** | Any third-party that processes personal data on our behalf must have a signed DPA. See [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md). |
| **Cross-border transfers** | Personal data transferred outside the EEA requires an approved mechanism (e.g., Standard Contractual Clauses). Flag to Legal before implementing. |
| **Retention limits** | Personal data must not be retained longer than necessary. Define and enforce retention periods per data type. |

---

### CCPA — California Consumer Privacy Act

**Applies to:** Organizations that meet thresholds for revenue, data volume, or data sales and serve California residents.

**Status:** ☐ Applies / ☐ Does not apply / ☐ Under review

#### Engineering Obligations

| Requirement | What Engineering Must Do |
|---|---|
| **Right to know** | Disclose what personal data is collected and for what purpose. |
| **Right to delete** | Honor deletion requests within 45 days. |
| **Right to opt out of sale** | If data is sold or shared for cross-context behavioral advertising, provide an opt-out mechanism. |
| **Non-discrimination** | Do not penalize users who exercise their CCPA rights. |

---

### SOC 2 — Service Organization Control 2

**Applies to:** Organizations that store, process, or transmit customer data in cloud-hosted services.

**Status:** ☐ Applies / ☐ Does not apply / ☐ Under review

**Trust Service Criteria relevant to engineering:**

| Criterion | Engineering Controls |
|---|---|
| **Security** | Access controls, encryption, vulnerability management, incident response. See [`standards/security-standards.md`](../standards/security-standards.md). |
| **Availability** | Defined uptime commitments, monitoring, incident response SLAs. See [`standards/observability-standards.md`](../standards/observability-standards.md). |
| **Confidentiality** | Data classification and handling rules. See [`compliance/data-classification.md`](./data-classification.md). |
| **Processing Integrity** | Accurate and complete processing; error handling; audit logs. |

SOC 2 audits require evidence collection. Engineering must maintain:
- Access logs with actor identity for all production systems.
- Change management records (PRs, approvals, deployment history).
- Vulnerability scan reports and remediation timelines.

---

### PCI-DSS — Payment Card Industry Data Security Standard

**Applies to:** Systems that store, process, or transmit cardholder data (credit/debit card numbers, CVVs, PINs).

**Status:** ☐ Applies / ☐ Does not apply / ☐ Under review

**Engineering obligations (if applicable):**
- Cardholder data must never be stored by this organization's systems unless explicitly scoped and audited for PCI compliance.
- Prefer payment tokenization through a PCI-certified payment processor (e.g., Stripe) — let the processor handle raw card data.
- Any new payment feature must be reviewed by the compliance team before implementation.

---

### HIPAA — Health Insurance Portability and Accountability Act

**Applies to:** Systems that create, receive, maintain, or transmit Protected Health Information (PHI) for covered entities or business associates.

**Status:** ☐ Applies / ☐ Does not apply / ☐ Under review

**Engineering obligations (if applicable):**
- PHI must be treated as Restricted data (see [`data-classification.md`](./data-classification.md)).
- Access to PHI must be logged with actor identity.
- All HIPAA-related features require Legal review before implementation.

---

## How to Flag a Compliance Question

If you encounter a scenario where compliance requirements are unclear:

1. **Stop.** Do not implement until the question is resolved.
2. Post in the team channel and tag the compliance reviewer.
3. Document the question and resolution in the relevant PR or ADR.

For urgent compliance questions during an incident, contact the Team Lead directly — see [`context/team-and-ownership.md`](../context/team-and-ownership.md).

---

## Related Documents

- [`compliance/data-classification.md`](./data-classification.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md)
- [`runbooks/incident-response.md`](../runbooks/incident-response.md)
- [`policies/data-handling-policy.md`](../policies/data-handling-policy.md)
