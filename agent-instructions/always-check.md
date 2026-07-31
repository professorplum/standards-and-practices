# Always-Check: Pre-Proposal Checklist for AI Agents

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Purpose

Before proposing or implementing any solution, an AI agent **must** work through this checklist. These are non-negotiable. If a proposed solution would violate any item, the agent must either revise the proposal or explicitly flag the conflict and stop for human review.

This checklist is intentionally short. The goal is not to slow agents down — it is to ensure every proposal is grounded in organizational reality.

---

## 1. Security

- [ ] Does the proposal comply with all requirements in [`standards/security-standards.md`](../standards/security-standards.md)?
  - **Authentication & Authorization:** Is auth required? Is least-privilege applied?
  - **Secrets:** Are secrets handled via the secrets manager — not hardcoded, not in environment variables in production, not logged?
  - **Input validation:** Is all external input validated and sanitized at the boundary?
  - **Transport:** Is TLS required? Are there any plain-HTTP calls?
  - **Data protection:** Is sensitive data encrypted at rest? Is PII minimized and handled per policy?
- [ ] If the proposal touches a new attack surface or handles sensitive data, has threat modeling been noted as required?

**If any security check fails:** Stop. Revise the proposal or flag the conflict explicitly. Security requirements are not negotiable.

---

## 2. Technology Selection

- [ ] Are all proposed technologies (languages, frameworks, libraries, cloud services, SaaS tools) in [`tech-radar/`](../tech-radar/)?
  - Check [`tech-radar/banned-and-restricted.md`](../tech-radar/banned-and-restricted.md) first. If a technology is on the banned list, it cannot be used — period.
  - If a technology is "restricted," review the stated conditions before using it.
  - If a technology is not listed at all, treat it as unapproved and flag it for human review.
- [ ] If a non-canonical technology is necessary, has it been noted that an ADR is required before implementation?

---

## 3. Compliance & Data Handling

- [ ] Does the proposal handle any personal data (PII)?
  - If yes: review [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md) for applicable requirements.
  - If yes: classify the data per [`compliance/data-classification.md`](../compliance/data-classification.md) and confirm handling rules are met.
- [ ] Does the proposal involve data retention, deletion, or transfer across borders?
  - If yes: flag for human review against compliance requirements.
- [ ] Does the proposal involve a new vendor or third-party service?
  - If yes: check [`tech-radar/third-party-services.md`](../tech-radar/third-party-services.md). If not listed, the vendor approval process in [`policies/vendor-approval-policy.md`](../policies/vendor-approval-policy.md) must be followed first.

---

## 4. Existing Services & Duplication

- [ ] Does the proposal introduce something that already exists?
  - Check [`context/service-catalog.md`](../context/service-catalog.md) for existing services, APIs, and capabilities before proposing a new one.
  - Prefer extending an existing service over creating a new one when the scope is narrow.
- [ ] Does the proposal assume a service or system exists that may not?
  - Verify against the service catalog before referencing external dependencies.

---

## 5. Architecture Alignment

- [ ] Does the proposal follow the appropriate reference architecture?
  - For a new HTTP service: [`reference-architectures/rest-api-service.md`](../reference-architectures/rest-api-service.md)
  - For an event-driven service: [`reference-architectures/event-driven-service.md`](../reference-architectures/event-driven-service.md)
- [ ] If the proposal deviates from a reference architecture, is the deviation justified and documented (or noted that an ADR is required)?
- [ ] Does the proposal fit within the environment topology described in [`context/environment-topology.md`](../context/environment-topology.md)?

---

## 6. Boundaries of Autonomous Action

- [ ] Is the proposed action within autonomous boundaries per [`autonomous-boundaries.md`](./autonomous-boundaries.md)?
  - If not, stop and present the proposal for human approval before proceeding.

---

## 7. Language & Terminology

- [ ] Does the proposal use terminology consistent with [`context/domain-glossary.md`](../context/domain-glossary.md)?
  - If a term is used differently, flag it to avoid confusion.

---

## How to Use This Checklist

1. Work through each section mentally before finalizing a proposal.
2. If an item is clearly not applicable (e.g., a documentation-only change has no technology choices), mark it N/A with a one-line justification.
3. If an item fails, do one of:
   - **Revise the proposal** to comply, then re-check.
   - **Explicitly flag the conflict** in your response and request human guidance before proceeding.
4. Do not assume a check passes because it was not explicitly raised by the human prompt.

---

## Related Documents

- [`autonomous-boundaries.md`](./autonomous-boundaries.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`tech-radar/`](../tech-radar/)
- [`compliance/`](../compliance/)
- [`context/service-catalog.md`](../context/service-catalog.md)
