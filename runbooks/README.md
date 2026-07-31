# Runbooks

**Purpose:** Step-by-step operational playbooks for predictable situations. When something goes wrong — or needs to happen carefully — a runbook removes ambiguity and reduces the blast radius of human error.

---

## Contents

| Runbook | When to Use |
|---|---|
| [`incident-response.md`](./incident-response.md) | A service is degraded, down, or a security incident is suspected |
| [`deployment-runbook.md`](./deployment-runbook.md) | Deploying a release to staging or production |
| [`rollback-runbook.md`](./rollback-runbook.md) | A deployment caused a regression and must be reversed |
| [`on-call-guide.md`](./on-call-guide.md) | Starting an on-call rotation; what tools to use and how |

---

## Principles

- **Runbooks describe process, not implementation.** They should be intelligible to any senior engineer even without deep familiarity with the specific system.
- **Keep them current.** A stale runbook is worse than no runbook — it builds false confidence. Update runbooks when the system changes.
- **Link, don't duplicate.** When a step requires following another runbook, link to it rather than copying the steps.
- **Write for stress.** Runbooks are read under pressure. Use numbered steps, clear checkpoints, and explicit decision points ("If X, go to step Y; otherwise continue").

---

## Maintenance

Owner: Platform / DevOps  
Last Reviewed: 2026-07-31

Review all runbooks after every major incident and at least quarterly. File issues for any runbook that is out of date.

---

## Related Documents

- [`context/team-and-ownership.md`](../context/team-and-ownership.md)
- [`policies/deployment-policy.md`](../policies/deployment-policy.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
