# Documentation Standards

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering  
**Last Updated:** 2026-07-31

---

## Overview

Good documentation reduces onboarding time, prevents repeated mistakes, and makes systems maintainable. These standards define what must be documented, how it should be structured, and where it should live.

---

## What Must Be Documented

| Artifact | Required Documentation |
|---|---|
| Public API | OpenAPI/Swagger spec or equivalent |
| Service / application | Architecture overview, setup guide, runbook |
| Library / package | README with installation, usage, and API reference |
| Decision | Architecture Decision Record (ADR) in `decision-logs/` |
| Process / policy | Document in `policies/` or `guides/` |
| Data schema change | Migration notes and updated data dictionary |

---

## README Requirements

Every repository and top-level directory must have a `README.md` that includes:

1. **What** — a one-paragraph description of purpose.
2. **Why** — context on why this exists.
3. **How** — getting started / setup instructions.
4. **Structure** — a brief map of the directory layout (for repos).
5. **Links** — pointers to related documentation.

---

## Writing Style

- Use plain language. Prefer short sentences.
- Use active voice: "Run `make test`" not "The tests can be run with `make test`".
- Use second person: "You should…" or just imperative: "Configure the…"
- Spell out acronyms on first use: "Architecture Decision Record (ADR)".
- Use numbered lists for sequential steps and bulleted lists for unordered items.

---

## Markdown Formatting

- Use ATX-style headings (`#`, `##`, `###`) — not underline style.
- Use fenced code blocks with language identifiers:
  ````
  ```bash
  npm install
  ```
  ````
- Tables must have a header row and alignment separators.
- Add blank lines before and after headings, code blocks, and lists.
- Keep line length under 120 characters where possible.

---

## Diagrams

- Prefer text-based diagrams (Mermaid, PlantUML, ASCII) that can be version-controlled.
- Store generated image exports alongside the source file.
- Diagrams must include a title and a brief caption explaining what they show.

---

## Versioning Documentation

- Major changes to documents require a version bump and a changelog entry at the top or bottom of the document.
- Use the `Last Updated` field in document front matter.
- Link to the previous version in the decision log if a document replaces an earlier one.

---

## Review & Maintenance

- Documentation must be reviewed whenever the system it describes changes.
- Stale documentation is treated with the same urgency as a bug.
- Each document has an **Owner** responsible for keeping it accurate.

---

## Related Documents

- [Contribution Guide](../guides/contribution-guide.md)
- [Decision Log Template](../decision-logs/template.md)
- [Coding Standards](./coding-standards.md)
