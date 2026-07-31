# Policies

This directory contains the organization's operational policies. Policies define *what must happen* under specific circumstances. They are mandatory and enforced.

## Contents

| Document | Description |
|---|---|
| [Code Review Policy](./code-review-policy.md) | Approval requirements, reviewer responsibilities, and author obligations for all PRs |
| [Branching Policy](./branching-policy.md) | Branch naming, lifetime rules, commit message format, and protected branch rules |
| [Deployment Policy](./deployment-policy.md) | Environment definitions, deployment gates, deploy windows, and rollback requirements |

## Proposing a New Policy

1. Draft the policy using the structure of an existing document.
2. Open a pull request and tag the domain owner and at least two senior engineers.
3. Policies require broad consensus — gather input before drafting to reduce revision cycles.
4. After merge, communicate the policy via the engineering channel and update the [main README](../README.md).

## Policy Exceptions

Every policy may grant exceptions in documented circumstances. To request an exception:

1. Document the specific policy section, the reason for exception, and the mitigating controls.
2. Get written approval from the domain owner.
3. Record the exception and its expiry in the relevant ticket or incident log.
