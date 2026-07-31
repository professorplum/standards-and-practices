# Decision-Making Framework

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership  
**Last Updated:** 2026-07-31

---

## Overview

Good decisions are made intentionally, with the right people involved, at the right time, and are documented so future teams can understand the context. This framework defines the process for making and recording technical and organizational decisions.

---

## Decision Categories

| Category | Examples | Decision Owner | Documentation Required |
|---|---|---|---|
| **Architecture** | Service boundaries, data storage, messaging | Tech Lead / Architect | ADR (required) |
| **Technology Selection** | Language, framework, library, cloud provider | Tech Lead + Team | ADR (required) |
| **Process** | Branching model, deploy cadence, oncall rotation | Engineering Manager | Policy or Guide update |
| **Design** | API contract, schema design, UI patterns | Feature owner + reviewer | PR description + design doc |
| **Operational** | Incident response, monitoring thresholds | DevOps / SRE | Runbook update |

---

## The DACI Model

For significant decisions, use the **DACI** model to clarify roles:

| Role | Meaning | Responsibility |
|---|---|---|
| **D** — Driver | The person moving the decision forward | Gathers input, facilitates discussion, sets deadline |
| **A** — Approver | The person who makes the final call | One person only (avoids deadlock) |
| **C** — Contributor | Subject matter experts providing input | Provide information and opinions; no veto |
| **I** — Informed | Stakeholders who must be notified of the outcome | Receive the decision; do not participate in making it |

---

## Decision Process

### Step 1 — Define the Problem
Write a clear, one-paragraph problem statement. Include:
- What is the current state?
- What is wrong or insufficient about it?
- What is the desired outcome?

### Step 2 — Identify Decision Criteria
List the factors that matter (e.g., cost, performance, team familiarity, vendor lock-in, security, maintainability). Assign relative weight if helpful.

### Step 3 — Generate Options
Identify at least two realistic options. Include a "do nothing" option when applicable.

### Step 4 — Evaluate Options
For each option, assess it against the criteria. Be explicit about trade-offs — there is rarely a perfect option.

### Step 5 — Make and Record the Decision
- Choose the option that best satisfies the criteria.
- Record the decision in an [ADR](../decision-logs/template.md).
- Communicate to all **Informed** parties.

### Step 6 — Review
- Decisions should be reviewed if significant new information emerges.
- A decision is superseded (not deleted) when it changes — record the new decision and link back.

---

## Architecture Decision Records (ADRs)

All Architecture and Technology Selection decisions must be recorded as ADRs in [`decision-logs/`](../decision-logs/). See the [ADR template](../decision-logs/template.md) for the required format.

ADR statuses:

| Status | Meaning |
|---|---|
| `Proposed` | Under discussion — not yet decided |
| `Accepted` | Decision made and in effect |
| `Superseded` | Replaced by a later ADR (link to replacement) |
| `Deprecated` | No longer applicable; not replaced |
| `Rejected` | Formally considered and declined |

---

## Anti-Patterns to Avoid

- **HiPPO** (Highest Paid Person's Opinion) — decisions by seniority alone, without evaluation.
- **Analysis paralysis** — seeking perfection at the cost of progress.
- **Undocumented decisions** — verbal-only decisions that leave no trace for future teams.
- **Revisiting settled decisions** without new information.

---

## Related Documents

- [ADR Template](../decision-logs/template.md)
- [Decision Logs](../decision-logs/)
- [Architecture Framework](./architecture-framework.md)
