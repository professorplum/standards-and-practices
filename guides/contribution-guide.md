# Contribution Guide

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Overview

This guide covers how to contribute to any repository in this organization — from opening an issue through to merging a pull request.

---

## Before You Start

1. **Find or create an issue.** All non-trivial work should be tracked in the issue tracker. If the issue doesn't exist, create it and get acknowledgment before writing code.
2. **Check for existing work.** Search open and closed PRs to avoid duplicating effort.
3. **Ask questions early.** If the requirements are unclear, ask in the issue before spending time on an approach that may be rejected.

---

## Setting Up Your Environment

```bash
# 1. Fork and clone the repository
git clone https://github.com/<your-org>/<repo>.git
cd <repo>

# 2. Install dependencies (adjust for your tech stack)
npm install          # Node.js
pip install -e .     # Python
go mod download      # Go

# 3. Verify the setup by running tests
npm test
pytest
go test ./...

# 4. Copy example environment config
cp .env.example .env
```

Refer to the repository's own `README.md` for project-specific setup steps.

---

## Branching

Follow the [Branching Policy](../policies/branching-policy.md):

```bash
# Create a branch from main
git checkout main
git pull origin main
git checkout -b feature/PROJ-123-short-description
```

---

## Making Changes

- Keep changes small and focused. One logical change per PR.
- Follow the [Coding Standards](../standards/coding-standards.md).
- Run the linter and formatter before committing:
  ```bash
  npm run lint
  npm run format
  ```
- Write or update tests for every change. Aim for meaningful coverage, not just hitting the percentage threshold.

---

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) format (see [Branching Policy](../policies/branching-policy.md#commit-messages)):

```
feat(auth): add password reset via email

Closes: PROJ-123
```

---

## Opening a Pull Request

1. Push your branch: `git push origin feature/PROJ-123-short-description`
2. Open a PR against `main` (or the appropriate base branch).
3. Fill in the PR template completely:
   - **What changed** — a clear summary.
   - **Why** — motivation and context.
   - **How to test** — steps for the reviewer to verify the change.
   - **Related issue** — link to the ticket.
4. Assign yourself and request reviewers.
5. Ensure all CI checks pass before requesting review.

---

## Responding to Review

- Read all comments before responding. Some comments may address the same underlying issue.
- For each `[BLOCKING]` comment: make the change, then resolve the thread.
- For each `[SUGGESTION]`: either implement it or explain your reasoning for declining.
- For each `[QUESTION]`: answer it in the thread.
- Push additional commits to address feedback — do not force-push after review has begun.
- Once all blocking issues are resolved, re-request review.

---

## Merging

- The PR author merges (not the reviewer) once all approvals are obtained and CI passes.
- Use **Squash and Merge** for feature branches (keeps `main` history clean).
- Use **Merge Commit** for release branches (preserves context).
- Delete the branch after merge.

---

## After Merging

- Close the related issue if not auto-closed by the PR.
- Monitor the staging deployment for errors.
- If the change introduced a new process or decision, update the relevant document in this repository.

---

## Related Documents

- [Branching Policy](../policies/branching-policy.md)
- [Code Review Policy](../policies/code-review-policy.md)
- [Coding Standards](../standards/coding-standards.md)
- [Onboarding Guide](./onboarding-guide.md)
