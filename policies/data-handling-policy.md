# Data Handling Policy

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering / Legal  
**Last Updated:** 2026-07-31

---

## Purpose

This policy defines how the organization collects, stores, shares, retains, and deletes data. It operationalizes the requirements in [`standards/security-standards.md`](../standards/security-standards.md) and [`compliance/data-classification.md`](../compliance/data-classification.md).

---

## Core Rules

1. **Classify before handling.** Every dataset must be assigned a classification level: Public, Internal, Confidential, or Restricted.
2. **Minimize data.** Collect and retain only what is necessary for the task and the applicable legal basis.
3. **Protect according to classification.** Use the controls defined in the data classification framework.
4. **Do not use production data in non-production environments** without approval and anonymization.
5. **Delete when the purpose expires.** Retention must be defined and enforced. When data is no longer needed, it must be deleted securely.
6. **Do not share with third parties** unless the sharing is approved and governed by a contract (e.g., DPA) where required.

---

## Handling Requirements by Classification

| Classification | Storage | Sharing | Retention |
|---|---|---|---|
| Public | No special restrictions | May be shared openly | As needed |
| Internal | Access-restricted systems | Internal only, unless approved | As needed, but defined |
| Confidential | Encrypted at rest; least privilege | Limited to approved personnel/systems | Define and enforce |
| Restricted | Encrypted at rest; strict access controls; no logging of raw values | Only approved, need-to-know, with legal/compliance review | Minimum necessary; delete promptly |

---

## PII and Personal Data

- PII is at least Confidential and may be Restricted depending on sensitivity.
- Do not collect PII unless there is a documented purpose and lawful basis.
- Use masking or tokenization where possible.
- Data subject requests (access, deletion, correction) must be handled through the defined process.

---

## Third-Party Sharing

Before transmitting data to a third-party service or vendor:
1. Confirm the data classification and the minimum data set required.
2. Verify the vendor is approved in [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md) or is going through the vendor approval process.
3. Ensure a DPA or equivalent agreement exists where required.
4. Document the data flow and retention terms.

---

## Enforcement

Violations of this policy may result in access removal, incident escalation, or formal disciplinary action. Security-sensitive or compliance-sensitive issues should be escalated immediately per [`runbooks/incident-response.md`](../runbooks/incident-response.md).

---

## Related Documents

- [`compliance/data-classification.md`](../compliance/data-classification.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`policies/vendor-approval-policy.md`](./vendor-approval-policy.md)
