# Branching Policy

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Purpose

A consistent branching strategy keeps the repository navigable, reduces merge conflicts, and makes deployments predictable.

---

## Branching Model

This organization uses a **trunk-based development** model with short-lived feature branches.

```
main  ←── feature/PROJ-123-add-login
      ←── bugfix/PROJ-456-fix-null-pointer
      ←── hotfix/PROJ-789-patch-auth-bypass
      ←── release/2.3.0
```

### Branch Types

| Branch | Pattern | Purpose | Lifetime |
|---|---|---|---|
| `main` | `main` | Always deployable trunk | Permanent |
| Feature | `feature/<ticket>-<slug>` | New functionality | Until merged (max 5 days) |
| Bugfix | `bugfix/<ticket>-<slug>` | Non-urgent bug fixes | Until merged |
| Hotfix | `hotfix/<ticket>-<slug>` | Urgent production fixes | Until merged (hours) |
| Release | `release/<semver>` | Stabilization / release prep | Until release cut |
| Experiment | `experiment/<slug>` | Spikes / throwaway exploration | Until abandoned or merged |

---

## Naming Rules

- All branch names must be lowercase and use hyphens as word separators.
- Include the ticket/issue number when one exists: `feature/PROJ-42-user-avatar-upload`.
- Avoid personal names in branch names; prefer ticket-based or descriptive names.
- Maximum length: 60 characters.

---

## Branch Lifecycle Rules

- Feature branches must not live longer than **5 business days**. Split large changes into smaller PRs.
- Branches that have had no commits for **14 days** will be automatically deleted by CI cleanup.
- Do not work directly on `main`. All changes require a pull request.
- Rebase or merge `main` into long-running branches at least every 2 days to reduce conflict debt.

---

## Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`, `perf`, `revert`

**Examples:**

```
feat(auth): add OAuth2 login with Google

fix(cart): prevent double-charge on retry

docs(readme): update setup instructions for Docker

chore(deps): bump lodash from 4.17.20 to 4.17.21
```

- Summary line must be 72 characters or fewer.
- Use imperative mood: "add feature" not "added feature".
- Reference the ticket in the footer: `Refs: PROJ-123` or `Closes: PROJ-456`.

---

## Protected Branches

The following branches are protected and require PRs:

- `main`
- `release/*`

Direct pushes to protected branches are blocked. Force-push is disabled. See [Code Review Policy](./code-review-policy.md) for approval requirements.

---

## Tags & Releases

- Production releases are tagged on `main` using semantic versioning: `v<major>.<minor>.<patch>`.
- Tags are created by the CI/CD pipeline after a successful release merge — not manually.
- Hotfix releases increment the patch version.

---

## Related Documents

- [Code Review Policy](./code-review-policy.md)
- [Deployment Policy](./deployment-policy.md)
