# Data Classification

**Version:** 1.0  
**Status:** Active  
**Owner:** Legal / Engineering  
**Last Updated:** 2026-07-31

---

## Purpose

Every piece of data this organization handles must be classified before it is stored, processed, or transmitted. Classification drives encryption requirements, access controls, retention policies, and breach notification obligations.

**AI agents:** Before proposing a design that involves data storage or transmission, classify the data using this framework and apply the corresponding handling rules.

---

## Classification Levels

### 🔓 Public

**Definition:** Data that is explicitly intended for public consumption. Disclosure has no business, legal, or reputational risk.

**Examples:** Marketing website content, public API documentation, open-source code, publicly available product information.

**Handling Rules:**
- No encryption at rest required (though TLS in transit is still required for all endpoints).
- No access restrictions.
- May be cached freely, indexed by search engines, shared openly.

---

### 🔵 Internal

**Definition:** Data intended for use within the organization. Not sensitive, but not meant for public sharing. Accidental disclosure is low-risk but undesirable.

**Examples:** Internal documentation, non-sensitive business metrics, team wikis, deployment configs that don't contain secrets.

**Handling Rules:**
- TLS required in transit.
- Access limited to authenticated employees or systems.
- Must not be shared externally without explicit approval.
- May be stored in internal systems without additional encryption at rest (standard disk encryption of infrastructure is sufficient).

---

### 🟡 Confidential

**Definition:** Data whose unauthorized disclosure could cause meaningful harm — business, legal, or reputational. Includes most customer data.

**Examples:** Customer PII (names, emails, addresses, phone numbers), business contracts, financial records, internal strategic plans, employee data, aggregate usage data that could identify customers.

**Handling Rules:**
- **Encrypt at rest** (AES-256 or equivalent) — application-level or database-level encryption for the most sensitive fields.
- **TLS required in transit** (TLS 1.2 minimum, TLS 1.3 preferred).
- Access limited to systems and personnel with a documented need.
- **Logs must not contain Confidential data** — mask or omit before logging.
- **Must not be sent to third-party services** without a signed Data Processing Agreement.
- Retention limits must be defined and enforced — data must be deleted when no longer needed.
- Breaches involving Confidential data must be reported to the compliance team within **24 hours**.

---

### 🔴 Restricted

**Definition:** The most sensitive data. Unauthorized disclosure could cause severe harm — regulatory penalties, significant legal exposure, serious harm to individuals, or critical security compromise.

**Examples:** Payment card data (PAN, CVV), government IDs, health/medical records (PHI), authentication credentials (passwords, private keys, tokens), biometric data, data under active legal hold.

**Handling Rules:**
- **Encrypt at rest** — mandatory, with key management via the secrets manager (never stored alongside the data).
- **TLS 1.3 required** in transit; no exceptions.
- Access is strictly need-to-know, with access logs for every access event.
- **Never log** Restricted data — not even masked/truncated versions unless explicitly approved.
- **Never store** Restricted data in dev or staging environments — use anonymized or synthetic substitutes.
- Payment card data: prefer tokenization through a PCI-certified processor; avoid storing raw card data.
- Credentials: use the secrets manager — never hardcode, never commit, never log.
- Retention: minimize retention; delete as soon as the purpose is fulfilled.
- Breaches involving Restricted data: notify the compliance team **immediately**, regardless of time of day.

---

## Classification Decision Tree

```
Is the data already public (on our website, open-source, press release)?
  → Yes: PUBLIC
  → No: Continue

Is the data exclusively internal (no customer or employee data, no secrets)?
  → Yes: INTERNAL
  → No: Continue

Does the data identify or could it be used to identify a specific person?
  → Yes: CONFIDENTIAL (minimum) — check next question

Is the data a credential, payment instrument, health record, or government ID?
  → Yes: RESTRICTED

Does the data carry specific regulatory obligations (GDPR, HIPAA, PCI-DSS)?
  → Yes: CONFIDENTIAL or RESTRICTED (consult applicable-regulations.md)
  → No: CONFIDENTIAL
```

---

## Handling PII Specifically

PII (Personally Identifiable Information) is always at least **Confidential**. Common PII fields and their minimum classification:

| Field | Minimum Classification |
|---|---|
| Name | Confidential |
| Email address | Confidential |
| Phone number | Confidential |
| Physical address | Confidential |
| IP address | Confidential (context-dependent) |
| Date of birth | Confidential |
| Government ID (SSN, passport) | Restricted |
| Financial account numbers | Restricted |
| Payment card data | Restricted |
| Health / medical data | Restricted |
| Biometric data | Restricted |
| Passwords / tokens / keys | Restricted |

---

## Data Handling in Non-Production Environments

Confidential and Restricted data from production **must not** be used in dev or staging environments.

- Use anonymized exports (replace real values with realistic fake data).
- Use synthetic data generators.
- If real data is absolutely required for a specific debugging scenario: get Team Lead approval, limit access, and delete the data when done.

---

## Related Documents

- [`compliance/applicable-regulations.md`](./applicable-regulations.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`policies/data-handling-policy.md`](../policies/data-handling-policy.md)
- [`context/environment-topology.md`](../context/environment-topology.md)
