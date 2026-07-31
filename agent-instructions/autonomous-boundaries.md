# Autonomous Boundaries for AI Agents

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Purpose

This document defines what an AI agent may do autonomously versus what requires explicit human approval. Clear boundaries allow agents to move fast on routine work while ensuring humans stay in the loop for consequential decisions.

The guiding principle: **act on well-defined, reversible tasks; pause on ambiguous, irreversible, or high-impact decisions.**

---

## Tier 1 — Fully Autonomous (no human approval needed)

Agents may perform these actions independently:

### Code & Implementation
- Implement a feature or fix described in a task/issue against a feature branch.
- Refactor code that does not change observable external behavior.
- Add, update, or fix tests.
- Update inline documentation, comments, and docstrings.
- Fix linting errors and formatting violations.
- Update dependencies to patch/minor versions within the approved version range.

### Research & Analysis
- Read any file in any repository the agent has access to.
- Search the codebase, documentation, or this repository.
- Summarize, explain, or critique existing code or proposals.
- Draft an ADR, design doc, or proposal for human review.

### Branch & Local Operations
- Create feature branches from `main`.
- Commit changes to a feature branch.
- Open a pull request for human review.

---

## Tier 2 — Propose & Wait for Approval

The agent must present a clear proposal and receive explicit human approval before proceeding:

### Architecture & Design
- Introducing a new service, database, or infrastructure component.
- Changing service boundaries or inter-service communication patterns.
- Adopting a technology not currently in the tech radar (even if not banned).
- Any deviation from a reference architecture.

### Data & Schema
- Creating or modifying production database schemas.
- Adding or changing data retention or deletion behavior.
- Adding new data collection that includes PII.

### Dependencies & Vendors
- Adding a new third-party service or vendor dependency.
- Upgrading a dependency to a major version.
- Adding a dependency with a non-permissive or unusual license.

### Branch & Merge Operations
- Merging to `main`, `release/*`, or `hotfix/*` branches.
- Creating or modifying branch protection rules.
- Force-pushing to any branch.

---

## Tier 3 — Human Only (agents must not perform, even if asked)

These actions are outside the scope of autonomous agent work regardless of instruction:

### Security & Secrets
- Accessing, storing, or transmitting production credentials, API keys, or secrets.
- Disabling security controls, authentication, or authorization checks.
- Modifying security policies, firewall rules, or IAM roles in production.

### Production Operations
- Executing commands directly against a production database.
- Deploying to production outside of the defined deployment pipeline.
- Deleting or archiving production data.

### Compliance & Legal
- Making commitments about regulatory compliance or legal obligations on behalf of the organization.
- Sharing code, data, or architectural details with external parties.

### Organizational Decisions
- Changing team ownership, on-call assignments, or escalation paths.
- Approving or rejecting an ADR on behalf of a human decision-maker.
- Resolving a disagreement between team members.

---

## When in Doubt

If an action is not clearly in Tier 1, treat it as Tier 2: present the proposal, explain the action and its consequences, and wait for approval before proceeding.

**Explicitly state your tier assessment** in your response when doing non-trivial work: "This is a Tier 1 task — I'll proceed autonomously" or "This requires Tier 2 approval — here is my proposed approach."

---

## Related Documents

- [`always-check.md`](./always-check.md)
- [`policies/branching-policy.md`](../policies/branching-policy.md)
- [`policies/deployment-policy.md`](../policies/deployment-policy.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
