# Security Standards

**Version:** 1.0  
**Status:** Active  
**Owner:** Security / Engineering  
**Last Updated:** 2026-07-31

---

## Overview

Security is a shared responsibility. These standards define minimum security requirements for all systems, services, and code produced by this organization. They apply to all environments: development, staging, and production.

---

## Authentication & Authorization

- **Authentication required** for all non-public endpoints.
- Use industry-standard protocols: OAuth 2.0 / OpenID Connect for user-facing auth, API keys or mTLS for service-to-service.
- Passwords must be hashed with bcrypt, argon2, or scrypt — never SHA-1/MD5.
- Enforce MFA for all human accounts with production access.
- Apply the **principle of least privilege**: grant only the permissions required for the task.
- Authorization checks must occur on the server side; never trust client-supplied roles or claims without verification.

---

## Secrets Management

- **Never commit secrets** (passwords, API keys, tokens, certificates) to source control — not even in private repos.
- Use a secrets manager (e.g., HashiCorp Vault, AWS Secrets Manager, GitHub Actions secrets) for all credentials.
- Rotate secrets on a schedule and immediately upon suspected exposure.
- Secrets must not be logged, printed, or included in error messages.
- Environment variables are acceptable for local development; use `.env.example` files with placeholder values only.

---

## Input Validation & Sanitization

- Validate and sanitize all input at the boundary (API, CLI, UI).
- Use an allowlist approach where possible (reject anything not explicitly permitted).
- Parameterize all database queries — never build SQL strings via concatenation.
- Escape output for the target context (HTML, JSON, shell) to prevent injection attacks (XSS, SQLi, command injection).

---

## Dependency Management

- Pin all dependency versions in production builds.
- Run `npm audit`, `pip-audit`, `govulncheck`, or equivalent on every CI run.
- Address **critical** and **high** severity CVEs within 7 days of disclosure.
- Address **medium** severity CVEs within 30 days.
- Remove unused dependencies.

---

## Transport Security

- All traffic must use TLS 1.2 or higher.
- Enforce HTTPS; redirect HTTP to HTTPS.
- Use HSTS headers in web applications.
- Do not use self-signed certificates in staging or production.

---

## Logging & Auditing

- Log all authentication events (success and failure).
- Log all authorization failures.
- Log significant data mutations (create, update, delete) with actor identity.
- Never log PII (email, phone, address, SSN, etc.) or secrets.
- Log retention must comply with applicable regulations (minimum 90 days).

---

## Data Protection

- Classify data before handling: Public, Internal, Confidential, Restricted.
- Encrypt Confidential and Restricted data at rest (AES-256 or equivalent).
- Apply data minimization: collect and store only what is necessary.
- Honor data deletion requests per applicable regulations (e.g., GDPR right to erasure).

---

## Vulnerability Management

- All production services must have a responsible disclosure contact.
- Perform threat modeling for new features that handle sensitive data or expose new attack surfaces.
- Security findings from automated scans (SAST, DAST, SCA) in CI must be reviewed within 5 business days.

---

## Incident Response

- Any suspected security incident must be reported to the security team immediately.
- Do not attempt self-contained remediation of an active breach without security team involvement.
- See the [Incident Response Runbook](../guides/incident-response-guide.md) for step-by-step procedures.

---

## Related Documents

- [Coding Standards](./coding-standards.md)
- [Deployment Policy](../policies/deployment-policy.md)
- [Decision Log: Secrets Manager Selection](../decision-logs/examples/adr-001-secrets-manager.md)
