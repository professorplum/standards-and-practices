# Runbook: On-Call Guide

**Version:** 1.0  
**Status:** Active  
**Owner:** Platform  
**Last Updated:** 2026-07-31

---

## Purpose

This guide explains the on-call rotation: what it means to be on-call, what tools to use, how to respond to alerts, and how to hand off.

---

## What On-Call Means

Being on-call means you are the first responder for production incidents during your shift. You are expected to:
- Acknowledge alerts within the SLA (15 minutes for P1, 30 minutes for P2).
- Investigate and attempt mitigation.
- Escalate appropriately — you are not expected to solve every problem alone.
- Keep `#incidents` updated during active incidents.

Being on-call does **not** mean:
- You must fix every problem yourself.
- You must be available for non-urgent requests.
- You cannot escalate to the team lead or another engineer.

---

## Rotation Structure

| Role | Rotation | Notes |
|---|---|---|
| Primary On-Call | Weekly, rotating through senior engineers | First to receive all alerts |
| Secondary On-Call | Weekly, offset by 3 days | Escalation target if primary doesn't acknowledge within 15 min |
| Team Lead | Permanent escalation | Paged for P1 incidents; available for P2 guidance |

Schedule is managed in [PagerDuty / OpsGenie — replace with your tool].

---

## Before Your Shift Starts

1. Confirm your contact info is current in the alerting tool.
2. Ensure your mobile device is set up for alert notifications.
3. Review the past week's incidents in `#incidents` — know what's been recently unstable.
4. Check for scheduled deployments during your shift (see `#deployments`).
5. Confirm a secondary on-call is assigned and reachable.

---

## Tools & Access

| Tool | Purpose | How to Access |
|---|---|---|
| PagerDuty / OpsGenie | Alert management and escalation | [link] |
| Grafana | Metrics dashboards | [link] |
| Kibana / Log viewer | Centralized log search | [link] |
| Kubernetes dashboard / kubectl | Pod and deployment status | Requires VPN + production cluster credentials |
| Secrets manager | Credential rotation during incidents | Requires break-glass access procedure |
| `#incidents` channel | Incident communication | Slack / Teams |

**Production access:** If you do not have production access set up, resolve this before your shift begins. Do not wait until an incident to discover access issues.

---

## Alert Response Process

1. **Acknowledge** the alert in PagerDuty / OpsGenie immediately.
2. **Post** in `#incidents` that you're investigating.
3. **Assess severity** using the table in [`incident-response.md`](./incident-response.md).
4. **Follow** the incident response runbook: [`incident-response.md`](./incident-response.md).

---

## Escalation

Escalate to Team Lead when:
- A P1 incident is not resolving within 30 minutes.
- You are unsure whether to roll back.
- The incident involves potential data loss or security exposure.
- You need someone to take over (fatigue, end of shift).

Escalation is expected and encouraged. There is no shame in escalating — the cost of delayed escalation is always higher.

---

## Handoff

At the end of your shift:
1. Ensure no open incidents are unresolved or unacknowledged.
2. Post a brief handoff summary in `#oncall`:
   ```
   On-call handoff: <your name> → <next person>
   
   Open incidents: <none / link to incident>
   Watch items: <anything that was flaky this week>
   Scheduled deploys: <any upcoming>
   ```
3. Transfer any open PagerDuty incidents to the incoming on-call.

---

## After an Incident

- For P1/P2: ensure a post-mortem is filed (see [`incident-response.md`](./incident-response.md)).
- For persistent or recurring alerts: file a reliability issue so it gets addressed in the next sprint.
- If the runbooks were insufficient or unclear: update them as part of your post-incident actions.

---

## Related Documents

- [`runbooks/incident-response.md`](./incident-response.md)
- [`runbooks/deployment-runbook.md`](./deployment-runbook.md)
- [`runbooks/rollback-runbook.md`](./rollback-runbook.md)
- [`context/team-and-ownership.md`](../context/team-and-ownership.md)
