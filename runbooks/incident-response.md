# Runbook: Incident Response

**Version:** 1.0  
**Status:** Active  
**Owner:** Platform / On-Call  
**Last Updated:** 2026-07-31

---

## Purpose

This runbook defines how to detect, triage, respond to, and recover from production incidents, including service degradations and suspected security events.

> This runbook is referenced by [`standards/security-standards.md`](../standards/security-standards.md). For security-specific incidents (breaches, exposed secrets), follow the **Security Incident** track below in addition to the general process.

---

## Severity Definitions

| Severity | Description | Response Time | Examples |
|---|---|---|---|
| **P1 — Critical** | Production down or severely degraded; broad customer impact | Acknowledge within 15 min; all-hands | Full service outage, data loss, security breach |
| **P2 — Major** | Significant feature broken; substantial customer impact | Acknowledge within 30 min; dedicated responder | Core feature unavailable, major performance degradation |
| **P3 — Minor** | Partial degradation; limited impact | Acknowledge within 2 hours | Edge case broken, non-critical feature down |
| **P4 — Low** | Cosmetic issue or near-miss; no customer impact | Next business day | Logging error, slow non-critical endpoint |

---

## General Incident Response Process

### Step 1 — Detect & Acknowledge

1. Alert received via PagerDuty / OpsGenie, or issue reported in `#incidents`.
2. On-call engineer acknowledges the alert **within the SLA for the severity**.
3. Post in `#incidents`: `"Acknowledging [P1/P2/P3] — [brief description]. Investigating."`

### Step 2 — Assess Severity

1. Check monitoring dashboards (Grafana, Prometheus).
2. Check recent deployments — was anything deployed in the last 2 hours? If yes, consider rollback first.
3. Assign severity (P1–P4) based on definitions above.
4. If P1: page Team Lead and VP Engineering immediately.

### Step 3 — Contain & Mitigate

**Goal: stop the bleeding, then understand the cause.**

1. If the incident was caused by a recent deployment, initiate rollback. See [`rollback-runbook.md`](./rollback-runbook.md).
2. If a specific feature is causing the problem, disable the feature flag if one exists.
3. If a downstream dependency is failing, activate the circuit breaker / fallback behavior if available.
4. Update `#incidents` with status every **15 minutes** during a P1, every **30 minutes** during a P2.

### Step 4 — Resolve

1. Identify root cause.
2. Implement fix (through normal deployment process if time permits; expedited review for hotfixes per [`policies/code-review-policy.md`](../policies/code-review-policy.md)).
3. Verify fix in staging before deploying to production when possible.
4. Deploy fix. Monitor for 30 minutes post-deployment to confirm resolution.

### Step 5 — Communicate

- For P1/P2: post a resolution message in `#incidents` and notify affected stakeholders.
- Update any public status page.
- If customers were affected, notify via the appropriate customer communication channel.

### Step 6 — Post-Mortem

- For P1 incidents: a written post-mortem is **required** within 5 business days.
- For P2 incidents: a post-mortem is strongly recommended.
- Post-mortems are blameless. Focus on systems, processes, and signals — not individuals.
- Post-mortem template: [link to template in your incident tracking system].

Post-mortem must include:
1. Incident timeline
2. Root cause
3. Impact (users affected, duration, data affected if any)
4. What went well
5. What went poorly
6. Action items (owner + due date for each)

---

## Security Incident Track

> Activate this track **in parallel** with the general process whenever a security incident is suspected.

**Signs of a security incident:** unexplained access, secrets potentially exposed, unexpected data access, phishing/social engineering, malware activity, suspicious API calls.

1. **Do not announce publicly** until the scope is understood — premature disclosure can alert an attacker.
2. **Contact the security team immediately** (see [`context/team-and-ownership.md`](../context/team-and-ownership.md)).
3. **Preserve evidence:** do not restart or wipe affected systems before forensics.
4. **Rotate any potentially exposed secrets immediately** — see [`standards/security-standards.md`](../standards/security-standards.md).
5. **Restrict access** to affected systems to a need-to-know group.
6. If PII or regulated data may have been accessed: legal and compliance must be notified. Regulatory notification timelines apply (e.g., 72-hour GDPR breach notification window). See [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md).

---

## Communication Templates

### Initial Alert (Slack / Teams)
```
🚨 [P1/P2] INCIDENT — <short description>
Status: Investigating
On-call: @<name>
Impact: <what users/services are affected>
Started: ~<time>
Next update: <time + 15 min>
```

### Resolution Message
```
✅ RESOLVED — <short description>
Duration: <start time> → <end time>
Root cause: <one sentence>
Fix applied: <what was done>
Post-mortem: <link / "pending" / "not required">
```

---

## Related Documents

- [`runbooks/rollback-runbook.md`](./rollback-runbook.md)
- [`runbooks/on-call-guide.md`](./on-call-guide.md)
- [`context/team-and-ownership.md`](../context/team-and-ownership.md)
- [`policies/code-review-policy.md`](../policies/code-review-policy.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`compliance/applicable-regulations.md`](../compliance/applicable-regulations.md)
