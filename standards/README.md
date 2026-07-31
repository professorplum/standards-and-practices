# Standards

This directory contains the organization's foundational technical standards. Standards define *how* work must be done. They are mandatory unless a documented exception is approved.

## Contents

| Document | Description |
|---|---|
| [Coding Standards](./coding-standards.md) | Naming, structure, testing, error handling, and review expectations for all code |
| [Security Standards](./security-standards.md) | Authentication, secrets, input validation, encryption, and incident requirements |
| [Documentation Standards](./documentation-standards.md) | What to document, how to write it, and where it belongs |

## Proposing a New Standard

1. Draft the standard using the structure in an existing document.
2. Open a pull request and tag the relevant owner team for review.
3. Standards require approval from at least two senior engineers and the relevant domain owner before merge.
4. After merge, announce the new standard in the engineering channel and update the [main README](../README.md).

## Updating an Existing Standard

1. Open a pull request with your proposed changes.
2. Describe the motivation for the change in the PR description.
3. Changes to existing standards require the same review process as new standards.
4. Bump the **Version** field and update the **Last Updated** date in the document.
