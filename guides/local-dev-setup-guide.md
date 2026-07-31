# Local Development Setup Guide

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering / Platform  
**Last Updated:** 2026-07-31

---

## Purpose

This guide helps a new engineer get a local development environment running quickly and consistently. It covers prerequisites, repository setup, local services, and common troubleshooting steps.

---

## Prerequisites

Before starting, ensure you have:
- Git
- Docker Desktop (or equivalent)
- A supported Node.js version (see [`tech-radar/languages-and-runtimes.md`](../tech-radar/languages-and-runtimes.md))
- Access to the organization's source repositories and secrets manager

---

## Initial Setup

1. Clone the repository and create a feature branch.
2. Copy any example environment files (for example, `.env.example` to `.env`) and fill in local placeholders.
3. Install dependencies using the project’s standard package manager.
4. Start local dependencies (database, cache, message broker) via Docker Compose.
5. Run the test suite or the service-specific smoke tests.

---

## Common Development Workflow

- Make changes in a feature branch.
- Run relevant unit and integration tests before submitting PRs.
- Use the local environment to validate the feature end-to-end.
- Do not use production data in local development.

---

## Troubleshooting

If something fails:
- Check the service logs and local containers first.
- Verify that your environment variables are set correctly.
- Confirm you are using the supported runtime version.
- Look for existing issues or runbooks before creating a new workaround.

---

## Related Documents

- [`guides/contribution-guide.md`](./contribution-guide.md)
- [`context/environment-topology.md`](../context/environment-topology.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
