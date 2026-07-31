# Runbook: Rollback

**Version:** 1.0  
**Status:** Active  
**Owner:** Platform / On-Call  
**Last Updated:** 2026-07-31

---

## Purpose

Step-by-step guide for reverting a production deployment when a release causes a regression or incident.

**Act fast.** The cost of a wrong rollback is low (re-deploy). The cost of a prolonged incident is high. When in doubt, roll back first and investigate after.

---

## When to Roll Back

Roll back immediately if, within 30 minutes of a deployment:
- Error rate increases by more than 5% above pre-deployment baseline.
- p99 latency increases by more than 50% above baseline.
- Core user flows are broken.
- Any `/healthz` or `/readyz` endpoint returns non-200.
- A P1 or P2 alert fires and the deployment is the likely cause.

Do **not** wait more than 5 minutes after recognizing a rollback trigger.

---

## Step 1 — Notify

1. Post in `#deployments` and `#incidents`:
   ```
   ⚠️ Rolling back <service> — <brief reason>.
   ```
2. Notify the on-call engineer if they are not already engaged.

---

## Step 2 — Identify the Previous Stable Version

1. Check the deployment pipeline run history (GitHub Actions) for the last successful production deployment.
2. Note the commit SHA or image tag of the previous stable release.
3. Confirm the previous version did not have the issue you are rolling back for. (Check deployment notes or the incident timeline.)

---

## Step 3 — Execute the Rollback

### Code Rollback

1. Trigger the production deployment pipeline with the previous stable image tag:
   ```
   # Via GitHub Actions manual trigger:
   # Workflow: deploy-production
   # Input: image_tag = <previous stable tag>
   ```
2. Monitor the pipeline to completion.

### Kubernetes Rollback (if pipeline is unavailable)

```bash
# Roll back to the previous revision
kubectl rollout undo deployment/<service-name> -n production

# Verify the rollout
kubectl rollout status deployment/<service-name> -n production

# Check running pods
kubectl get pods -n production -l app=<service-name>
```

---

## Step 4 — Verify Rollback

Within 5 minutes of rollback completion:

- [ ] `/healthz` returns 200.
- [ ] `/readyz` returns 200.
- [ ] Error rate has returned to pre-incident baseline.
- [ ] Latency has returned to pre-incident baseline.
- [ ] A quick smoke test of the primary user flow passes.

If metrics do **not** recover after rollback: the issue may not be deployment-related. Escalate to the incident response process — see [`incident-response.md`](./incident-response.md).

---

## Step 5 — Database Migration Rollback

**Database migrations often cannot be automatically rolled back.** If the problematic deployment included a schema migration:

1. **Do not attempt an automatic rollback if data has already been written to the new schema.**
2. Consult the DBA immediately.
3. Options (in order of preference):
   a. If no data written to new columns/tables: apply the down migration script if one exists.
   b. If data has been written: a forward-fix (new migration that returns to the desired state) is usually safer.
4. All database migration rollbacks require DBA involvement and a post-mortem.

---

## Step 6 — Post-Rollback

1. Post resolution in `#deployments` and `#incidents`:
   ```
   ✅ Rollback complete: <service> is now running <previous version>.
   Error rate / latency: nominal.
   ```
2. Confirm any on-call alerts are resolved or acknowledged.
3. File a post-mortem issue (required for any rollback that caused a P1/P2 incident).
4. The broken release **must not be re-deployed** until the root cause is identified and fixed.

---

## Related Documents

- [`runbooks/deployment-runbook.md`](./deployment-runbook.md)
- [`runbooks/incident-response.md`](./incident-response.md)
- [`policies/deployment-policy.md`](../policies/deployment-policy.md)
