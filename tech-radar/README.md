# Tech Radar

**Purpose:** The canonical authority on technology choices for this organization. Before introducing any technology — language, framework, library, database, cloud service, or third-party tool — check here.

---

## Structure

Technologies are categorized into four rings:

| Ring | Meaning |
|---|---|
| ✅ **Approved** | Standard choice. Use confidently. No additional justification needed. |
| 🔵 **Trial** | Approved for limited use with deliberate evaluation. Document findings. New production use requires team lead awareness. |
| ⚠️ **Restricted** | May be used only under specific conditions stated in the entry. Non-compliant use requires an ADR and team lead approval. |
| 🚫 **Banned** | Must not be used in new code. Existing uses must be tracked for removal. See [`banned-and-restricted.md`](./banned-and-restricted.md) for rationale. |

---

## Contents

| Document | What It Covers |
|---|---|
| [`languages-and-runtimes.md`](./languages-and-runtimes.md) | Programming languages, runtimes, package managers |
| [`data-stores.md`](./data-stores.md) | Databases, caches, search engines, object storage |
| [`infrastructure-and-cloud.md`](./infrastructure-and-cloud.md) | Cloud providers, orchestration, networking, observability tooling |
| [`third-party-services.md`](./third-party-services.md) | Approved SaaS products, APIs, and external vendors |
| [`banned-and-restricted.md`](./banned-and-restricted.md) | Explicit ban list with rationale — check this first |

---

## For AI Agents

1. **Check [`banned-and-restricted.md`](./banned-and-restricted.md) first.** If a technology is listed there, do not use it and do not suggest it.
2. If a technology is not listed in any radar file, treat it as **unapproved**. Flag it and request human review before including it in a proposal.
3. If you believe a technology should be added or moved, draft an ADR and note it as requiring human approval — do not self-approve.

---

## Updating the Radar

Technology ring changes require:
- An ADR documenting the rationale (see [`frameworks/decision-making-framework.md`](../frameworks/decision-making-framework.md))
- Team lead approval
- Update to the relevant radar file and this README

Owner: Engineering Leadership  
Last Reviewed: 2026-07-31
