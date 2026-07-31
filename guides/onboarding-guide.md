# Onboarding Guide

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Welcome

Welcome to the team. This guide will help you get up to speed on how we work, what tools we use, and where to find information. You should be able to complete these steps in your first week.

---

## Week 1 Checklist

### Access & Accounts
- [ ] GitHub organization membership granted
- [ ] Secrets manager access provisioned
- [ ] Cloud provider console access (read-only for prod, full for dev/staging)
- [ ] CI/CD pipeline access
- [ ] Monitoring / observability dashboards access
- [ ] Communication channels joined (`#engineering`, `#deployments`, `#incidents`)

### Reading List (Required)
- [ ] [Coding Standards](../standards/coding-standards.md)
- [ ] [Security Standards](../standards/security-standards.md)
- [ ] [Code Review Policy](../policies/code-review-policy.md)
- [ ] [Branching Policy](../policies/branching-policy.md)
- [ ] [Deployment Policy](../policies/deployment-policy.md)
- [ ] [Contribution Guide](./contribution-guide.md)
- [ ] [Architecture Framework](../frameworks/architecture-framework.md)
- [ ] [Reference Architectures](../reference-architectures/) — read the one(s) relevant to your team's systems

### Technical Setup
- [ ] Local development environment configured for at least one core service
- [ ] All tests passing locally
- [ ] Successfully created and merged a "hello world" PR (e.g., add your name to `CONTRIBUTORS.md`)

### People
- [ ] 1:1 with your manager scheduled
- [ ] Introduction to your team lead and on-call buddy
- [ ] Attended one team standup

---

## Key Concepts

### How We Work

We use **trunk-based development**: all engineers commit to short-lived feature branches and merge to `main` frequently (ideally daily). The [Branching Policy](../policies/branching-policy.md) covers the details.

Code review is required for all changes to `main`. The [Code Review Policy](../policies/code-review-policy.md) explains who reviews, what they check, and how to respond to feedback.

Deployments happen continuously to staging (on every merge to `main`) and manually to production during deploy windows. The [Deployment Policy](../policies/deployment-policy.md) covers the gates and process.

### Where Things Live

| What | Where |
|---|---|
| Application code | Service-specific repositories |
| Standards, policies, frameworks | This repository (`standards-and-practices`) |
| Reference architectures | [`reference-architectures/`](../reference-architectures/) in this repository |
| Architecture diagrams | `/docs/architecture/` in each service repo |
| Decision records | [`decision-logs/`](../decision-logs/) in this repository |
| Infrastructure | `infrastructure/` repository |
| Runbooks | Internal wiki or `guides/` in this repository |

### How Decisions Are Made

Significant technical decisions are documented as Architecture Decision Records (ADRs). See the [Decision-Making Framework](../frameworks/decision-making-framework.md) and the [ADR template](../decision-logs/template.md).

---

## Getting Help

- **Stuck on a technical problem?** Post in `#engineering` or ask your team lead.
- **Need access to something?** Ping your manager or the DevOps channel.
- **Found a bug in documentation?** Open a PR to fix it.
- **Unclear on a policy?** The document has an Owner field — reach out to them directly.
- **Security concern?** Contact the security team immediately. Do not discuss in public channels.

---

## Your First Contribution

1. Find a `good-first-issue` labeled ticket in the backlog.
2. Follow the [Contribution Guide](./contribution-guide.md) end to end.
3. Ask for a review from your team lead or on-call buddy.

This first contribution helps you verify your environment, practice the workflow, and get comfortable with code review.

---

## Related Documents

- [Contribution Guide](./contribution-guide.md)
- [Coding Standards](../standards/coding-standards.md)
- [Code Review Policy](../policies/code-review-policy.md)
