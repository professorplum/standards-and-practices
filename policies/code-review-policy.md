# Code Review Policy

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Purpose

Code review improves quality, spreads knowledge, and is the primary gate for catching bugs and standards violations before code reaches production. This policy defines who must review code, what reviewers must check, and how reviews must be conducted.

---

## Scope

This policy applies to all pull requests (PRs) targeting a protected branch (`main`, `master`, `release/*`, `hotfix/*`).

---

## Review Requirements

| Branch target | Minimum approvals | Conditions |
|---|---|---|
| `main` / `master` | 2 | At least one approval from a senior engineer or team lead |
| `release/*` | 2 | Must include the release manager |
| `hotfix/*` | 1 | Senior engineer or team lead; expedited process applies |
| Feature branches | 0 | Peer review encouraged but not gated |

- The author **cannot** approve their own PR.
- Approvals are invalidated by subsequent pushes; re-review is required.
- PRs that have been open for more than **5 business days** without activity must be pinged or closed.

---

## What Reviewers Must Check

### Correctness
- Does the code do what the PR description says it does?
- Are edge cases handled?
- Are error conditions handled without silent failures?

### Standards Compliance
- Does the code follow the [Coding Standards](../standards/coding-standards.md)?
- Are new secrets or credentials committed? (Block immediately)
- Does the code meet [Security Standards](../standards/security-standards.md)?

### Tests
- Are there tests for the new or changed behavior?
- Are tests meaningful (not just coverage padding)?
- Do tests pass in CI?

### Documentation
- Are public APIs documented?
- Is the PR description clear and complete?
- Do `README` files need updating?

### Scope
- Is the PR focused? (Single logical change)
- Are there unrelated changes that should be in a separate PR?

---

## Reviewer Etiquette

- Be specific and constructive. Explain *why* a change is needed.
- Distinguish blocking issues from non-blocking suggestions. Use prefixes:
  - `[BLOCKING]` — must be resolved before merge
  - `[SUGGESTION]` — optional improvement
  - `[QUESTION]` — seeking understanding, not requesting change
- Respond to review comments within **1 business day**.
- Resolve your own comments only after the requested change is made.
- Approve only if you would be comfortable merging yourself.

---

## Author Responsibilities

- Write a clear PR description: what changed, why, and how to test.
- Keep PRs small: under 400 lines of changed code where possible.
- Link to the related issue or ticket.
- Respond to all reviewer comments before requesting re-review.
- Do not merge until all `[BLOCKING]` comments are resolved and CI passes.

---

## Expedited Review (Hotfixes)

For production hotfixes:
1. Notify the on-call engineer and team lead in the incident channel.
2. One approval from a senior engineer or team lead is sufficient.
3. A follow-up review must be completed within 1 business day of merge.

---

## Enforcement

- Protected branches enforce the minimum approval counts via branch protection rules.
- Bypassing branch protection requires VP Engineering approval and must be logged in the incident log.

---

## Related Documents

- [Coding Standards](../standards/coding-standards.md)
- [Branching Policy](./branching-policy.md)
- [Deployment Policy](./deployment-policy.md)
