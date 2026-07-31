# Runbook: Deployment

**Version:** 1.0  
**Status:** Active  
**Owner:** Platform / Release Manager  
**Last Updated:** 2026-07-31

---

## Purpose

Step-by-step guide for deploying a release to staging and production. Follow this runbook for every production deployment.

---

## Prerequisites

Before starting a deployment:

- [ ] All target changes are merged to `main` and CI is green.
- [ ] Any required ADRs are written and accepted.
- [ ] Reviewer has confirmed the deployment is ready (PR approvals complete per [`policies/code-review-policy.md`](../policies/code-review-policy.md)).
- [ ] Release notes or a changelog entry is prepared.
- [ ] The on-call engineer is aware a deployment is happening.
- [ ] If a database migration is included: migration has been reviewed by a senior engineer and tested in staging.

---

## Step 1 — Deploy to Staging

1. Trigger the staging deployment pipeline (GitHub Actions workflow `deploy-staging`).
2. Wait for the pipeline to complete successfully.
3. Verify the deployment:
   - [ ] Check `/healthz` (liveness) and `/readyz` (readiness) endpoints return 200.
   - [ ] Smoke test the primary user flows manually or via automated smoke tests.
   - [ ] Check Grafana dashboards — error rate should not have increased.
   - [ ] If a database migration ran, verify the schema change is as expected.
4. If staging verification **fails**: do not proceed to production. Investigate and fix first.

---

## Step 2 — Production Deployment Window

- Preferred deployment window: **Tuesday–Thursday, 10:00–14:00 local time** (lowest traffic, business hours).
- Avoid: Fridays, day before holidays, during known peak traffic periods.
- For urgent hotfixes, see the expedited process in [`policies/code-review-policy.md`](../policies/code-review-policy.md#expedited-review-hotfixes).

---

## Step 3 — Deploy to Production

1. Post in `#deployments`: `"Starting production deploy: <service> <version> — <brief description of change>."`
2. Trigger the production deployment pipeline (GitHub Actions workflow `deploy-production`).
3. Monitor the pipeline — do not walk away during the deployment.
4. Watch the following for 15 minutes post-deployment:
   - [ ] Error rates (Grafana — target: no increase above baseline)
   - [ ] Latency / p99 (target: within 10% of pre-deployment baseline)
   - [ ] `/healthz` and `/readyz` endpoints still returning 200
   - [ ] Relevant business metrics (e.g., order creation rate) behaving normally
5. If any metric is degrading: initiate rollback immediately. See [`rollback-runbook.md`](./rollback-runbook.md). Do not wait to "see if it recovers."

---

## Step 4 — Post-Deployment

1. Post in `#deployments`: `"✅ Production deploy complete: <service> <version> — nominal."`
2. If database migration ran: notify the DBA to confirm production schema is as expected.
3. Close the deployment issue / ticket.
4. Update the `context/service-catalog.md` if the service's API or dependencies changed.

---

## Database Migration Deployments

Migrations that add columns, create tables, or add indexes are generally safe with the blue/green strategy. Migrations that remove columns or change types require extra care:

1. **Phase 1:** Deploy code that is compatible with both old and new schema.
2. **Phase 2:** Run the migration.
3. **Phase 3:** Deploy code that drops support for the old schema.

Never run a destructive migration and deploy new code in the same deployment step.

---

## Rollback Trigger Conditions

Initiate rollback if, within 30 minutes of deployment:
- Error rate increases by more than 5% above baseline.
- p99 latency increases by more than 50% above baseline.
- Any `/healthz` endpoint returns non-200.
- On-call engineer receives P1/P2 alerts attributable to this deployment.

See [`rollback-runbook.md`](./rollback-runbook.md) for the rollback procedure.

---

## Related Documents

- [`runbooks/rollback-runbook.md`](./rollback-runbook.md)
- [`runbooks/incident-response.md`](./incident-response.md)
- [`policies/deployment-policy.md`](../policies/deployment-policy.md)
- [`policies/code-review-policy.md`](../policies/code-review-policy.md)
