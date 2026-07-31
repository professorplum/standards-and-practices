# Reference Architectures

This directory contains canonical architectural patterns that teams can adopt for common system types. A reference architecture is an opinionated, pre-validated blueprint — it encodes the organization's best thinking on how to build a particular kind of system so teams don't start from scratch each time.

## What Belongs Here

A reference architecture answers the question: *"What does a well-built `<type of system>` look like in our organization?"*

It is more concrete than a framework and more general than a POC:

| Artifact | Scope | Audience | Example |
|---|---|---|---|
| **Framework** | Principles and process | Any decision | Architecture Framework |
| **Reference Architecture** | Canonical pattern for a system type | Teams starting a new system | REST API Service, Event-Driven Service |
| **POC Example** | Applied standards in a specific codebase | Reviewers / learners | `poc-examples/rest-api/` |
| **Standard** | Rules for all code | All engineers | Coding Standards |

## Contents

| Document | System Type | Status |
|---|---|---|
| [REST API Service](./rest-api-service.md) | Synchronous HTTP/REST backend service | Active |
| [Event-Driven Service](./event-driven-service.md) | Asynchronous, message-based service | Active |

## How to Use a Reference Architecture

1. Read the reference architecture for your system type before designing.
2. Adopt the pattern as your starting point.
3. Deviations are allowed but must be documented in an [ADR](../decision-logs/template.md) that explains why the standard pattern does not fit and what risks the deviation introduces.

## Adding a New Reference Architecture

1. Identify a system type the organization builds repeatedly.
2. Draft the reference architecture using the structure of an existing document (overview, component diagram, component descriptions, data flow, technology choices, cross-cutting concerns, known trade-offs).
3. Open a pull request; reference architectures require approval from at least two senior engineers and the Tech Lead or Architect.
4. Add an entry to the table above and to the [main README](../README.md).
