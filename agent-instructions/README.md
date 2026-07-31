# Agent Instructions

**Purpose:** This section is the entry point for AI agents (and the humans who prompt them) operating in this organization's codebase. Before proposing or implementing any solution, agents must consult the documents here.

---

## Why This Section Exists

This repository is organizational institutional memory. It captures decisions, constraints, standards, and context that are not visible in any single codebase. An agent that only reads source code is missing critical context — security requirements, compliance obligations, approved technologies, existing services, and the boundaries of autonomous action.

---

## Mandatory Pre-Work (Read First)

Before proposing or implementing **anything**, an agent must:

1. **Read [`always-check.md`](./always-check.md)** — non-negotiable checklist. Every proposal must satisfy it.
2. **Read [`autonomous-boundaries.md`](./autonomous-boundaries.md)** — know what you can do independently and what requires a human decision.

---

## Quick Reference: Where to Find Things

| Question | Go Here |
|---|---|
| "Is this approach secure?" | [`standards/security-standards.md`](../standards/security-standards.md) |
| "Is this technology approved?" | [`tech-radar/`](../tech-radar/) |
| "What regulations apply here?" | [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md) |
| "How should I classify/handle this data?" | [`compliance/data-classification.md`](../compliance/data-classification.md) |
| "What services already exist?" | [`context/service-catalog.md`](../context/service-catalog.md) |
| "What environments are there?" | [`context/environment-topology.md`](../context/environment-topology.md) |
| "What does this term mean?" | [`context/domain-glossary.md`](../context/domain-glossary.md) |
| "Who owns this?" | [`context/team-and-ownership.md`](../context/team-and-ownership.md) |
| "How should I structure this service?" | [`reference-architectures/`](../reference-architectures/) |
| "How do I deploy / roll back?" | [`runbooks/`](../runbooks/) |
| "What's the coding standard?" | [`standards/coding-standards.md`](../standards/coding-standards.md) |
| "How are ADRs made?" | [`frameworks/decision-making-framework.md`](../frameworks/decision-making-framework.md) |

---

## Related Documents

- [`always-check.md`](./always-check.md)
- [`autonomous-boundaries.md`](./autonomous-boundaries.md)
