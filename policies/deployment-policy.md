# Deployment Policy

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering / DevOps  
**Last Updated:** 2026-07-31

---

## Purpose

This policy defines how and when code changes are deployed to each environment, who is authorized to deploy, and what safety checks must pass before deployment proceeds.

---

## Environments

| Environment | Purpose | Who can deploy | Deploy trigger |
|---|---|---|---|
| `development` | Individual / team development | Any engineer | Automatic on push to feature branch |
| `staging` | Integration testing, demo, stakeholder review | Any engineer | Automatic on merge to `main` |
| `production` | Live service | On-call engineer + team lead | Manual approval in CI pipeline |

---

## Pre-Deployment Requirements

All of the following must pass before any deployment to **staging** or **production**:

- [ ] All CI checks pass (lint, unit tests, integration tests).
- [ ] Code review approvals obtained per the [Code Review Policy](./code-review-policy.md).
- [ ] No critical or high-severity security findings open in the security scanner.
- [ ] Database migrations have been reviewed and a rollback plan exists.
- [ ] Dependent services are compatible with the new version (no breaking API changes without versioning).

For **production** only:

- [ ] Staging deployment has been stable for at least **30 minutes**.
- [ ] Release notes have been drafted.
- [ ] On-call engineer has been notified and is available during the deploy window.

---

## Deploy Windows

| Environment | Deploy window | Exceptions |
|---|---|---|
| `development` | Any time | N/A |
| `staging` | Any time | N/A |
| `production` | Mon–Thu, 09:00–16:00 local time | Hotfixes (see below) |

Deployments to production outside the deploy window require VP Engineering approval.

---

## Deployment Process

1. Confirm pre-deployment requirements checklist above.
2. Trigger deployment from the CI/CD pipeline UI — do not deploy manually from a local machine.
3. Monitor the deployment dashboard and error rates for **15 minutes** post-deploy.
4. Confirm smoke tests pass in the target environment.
5. Post a deploy notification in `#deployments` with: service name, version, deployer, and any notable changes.

---

## Rollback

- Every deployment must be reversible within **10 minutes**.
- If error rates spike >1% above baseline within 30 minutes of deploy, initiate rollback immediately.
- The CI/CD pipeline provides a one-click rollback to the previous successful build.
- Database migrations that cannot be automatically rolled back must have a documented manual rollback procedure before deployment is approved.

---

## Hotfix Deployments

For urgent production issues:

1. Follow the [Branching Policy hotfix flow](./branching-policy.md).
2. Obtain approval from one senior engineer (expedited review).
3. Hotfixes may deploy outside the production deploy window with on-call engineer sign-off.
4. A post-incident review must be completed within **3 business days**.

---

## Configuration & Secrets

- Configuration values that differ between environments must be managed via environment variables or the secrets manager — never hardcoded.
- Production secrets must not be accessible in non-production environments.
- See [Security Standards – Secrets Management](../standards/security-standards.md#secrets-management).

---

## Enforcement

- The CI/CD pipeline enforces pre-deployment checks automatically.
- Manual deployments that bypass the pipeline are a policy violation and must be reported.

---

## Related Documents

- [Code Review Policy](./code-review-policy.md)
- [Branching Policy](./branching-policy.md)
- [Security Standards](../standards/security-standards.md)
