# standards-and-practices

This repository is the organization's institutional memory: the trusted context that humans and AI agents should consult before making technical or operational decisions. It combines standards, policies, frameworks, reference architectures, operational playbooks, compliance guidance, and implementation context.

---

## For AI Agents

Before proposing or implementing a change, start here:

- [Agent Instructions](./agent-instructions/README.md)
- [Always-Check Checklist](./agent-instructions/always-check.md)
- [Autonomous Boundaries](./agent-instructions/autonomous-boundaries.md)

If a proposal touches security, technology choices, compliance, or operational risk, consult the relevant section before responding.

---

## Repository Map

| Directory | Purpose |
|---|---|
| [`agent-instructions/`](./agent-instructions/) | Meta-guidance for AI agents — how to use this repo and what to check first |
| [`context/`](./context/) | Organizational and environment context — topology, service catalog, glossary, ownership |
| [`standards/`](./standards/) | Mandatory technical rules — coding, security, documentation, testing, observability |
| [`policies/`](./policies/) | Mandatory process rules — code review, branching, deployment, data handling, vendor approval |
| [`frameworks/`](./frameworks/) | Structured approaches and mental models — decision-making, architecture principles |
| [`reference-architectures/`](./reference-architectures/) | Canonical blueprints for common system types — REST API service, event-driven service |
| [`tech-radar/`](./tech-radar/) | Approved, trial, restricted, and banned technologies |
| [`runbooks/`](./runbooks/) | Operational playbooks for incident response, deployment, rollback, and on-call |
| [`compliance/`](./compliance/) | Regulatory and legal context — applicable regulations and data classification |
| [`guides/`](./guides/) | Step-by-step instructions — onboarding, contribution, incident response, local setup |
| [`decision-logs/`](./decision-logs/) | Architecture Decision Records (ADRs) — what was decided, why, and what was rejected |
| [`poc-examples/`](./poc-examples/) | Working code demonstrating standards and reference architectures in practice |

---

## Quick Links

- [Agent Instructions](./agent-instructions/README.md)
- [Always-Check Checklist](./agent-instructions/always-check.md)
- [Coding Standards](./standards/coding-standards.md)
- [Security Standards](./standards/security-standards.md)
- [Testing Standards](./standards/testing-standards.md)
- [Observability Standards](./standards/observability-standards.md)
- [Code Review Policy](./policies/code-review-policy.md)
- [Branching Policy](./policies/branching-policy.md)
- [Data Handling Policy](./policies/data-handling-policy.md)
- [Vendor Approval Policy](./policies/vendor-approval-policy.md)
- [REST API Reference Architecture](./reference-architectures/rest-api-service.md)
- [Event-Driven Service Reference Architecture](./reference-architectures/event-driven-service.md)
- [Decision-Making Framework](./frameworks/decision-making-framework.md)
- [Onboarding Guide](./guides/onboarding-guide.md)
- [Incident Response Guide](./guides/incident-response-guide.md)
- [ADR Template](./decision-logs/template.md)
