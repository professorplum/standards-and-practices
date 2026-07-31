# Context

**Purpose:** This section captures the current state of the organization's technical environment — the things that cannot be inferred from source code alone. It is required reading for AI agents before reasoning about any proposed change.

---

## Contents

| Document | What It Contains |
|---|---|
| [`environment-topology.md`](./environment-topology.md) | What environments exist, how they are structured, naming conventions |
| [`service-catalog.md`](./service-catalog.md) | What services/systems exist, their owners, APIs, and communication patterns |
| [`domain-glossary.md`](./domain-glossary.md) | Canonical definitions for domain terms — use these consistently |
| [`team-and-ownership.md`](./team-and-ownership.md) | Teams, who owns what, escalation contacts |

---

## For AI Agents

When asked to propose, implement, or reason about a change:

1. Check the **service catalog** before suggesting a new service — it may already exist.
2. Check **environment topology** before referencing deployment targets, URLs, or config conventions.
3. Use the **domain glossary** for all entity and concept names — consistency prevents confusion.
4. Check **team and ownership** to identify the right owner when a change crosses team boundaries.

---

## Maintenance

These documents reflect reality, not aspirations. When reality changes (a new service is added, a team reorganizes, an environment is decommissioned), **update these documents as part of the change** — not after.

Owner: Engineering Leadership  
Last Reviewed: 2026-07-31
