# Frameworks

This directory contains frameworks that provide structured approaches to recurring challenges. Frameworks offer a process or mental model rather than hard rules — they guide judgment.

## Contents

| Document | Description |
|---|---|
| [Decision-Making Framework](./decision-making-framework.md) | How to make, evaluate, and record significant decisions using the DACI model and ADRs |
| [Architecture Framework](./architecture-framework.md) | Architectural principles, quality attribute priorities, service and data design guidelines |

## Difference Between Frameworks, Reference Architectures, Standards, and Policies

| Type | Purpose | Mandatory? |
|---|---|---|
| **Standard** | Defines *how* work must be done (specific rules) | Yes |
| **Policy** | Defines *what must happen* in a given situation | Yes |
| **Framework** | Provides a process or model to guide judgment | Recommended |
| **Reference Architecture** | Canonical blueprint for a specific system type — lives in [`reference-architectures/`](../reference-architectures/) | Recommended baseline; deviations need an ADR |
| **Guide** | Step-by-step instructions for a specific task | Recommended |

## Adding a New Framework

1. Identify a recurring decision or problem that would benefit from a structured approach.
2. Draft the framework document, clearly distinguishing between required steps and judgment calls.
3. Open a pull request; frameworks require approval from at least one Engineering Lead.
