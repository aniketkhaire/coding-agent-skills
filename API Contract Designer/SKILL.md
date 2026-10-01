name: api-contract-designer
description: >-
  HTTP API design and review workflow. Define resources from use cases, design
  consistent endpoints, specify a machine-readable error model, choose a
  versioning strategy, and produce an OpenAPI spec with examples. Use when
  creating new endpoints, reviewing API changes, versioning, or designing
  pagination, filtering, and error conventions.
---

# API Contract Designer Guide

## Core API Design Principles
1. **Contracts Are Promises:** Once published, an endpoint is a commitment. Design for the stability you will have to maintain.
2. **Consistency Over Cleverness:** One naming scheme, one pagination style, one error shape across the whole API. Boring is good.
3. **Errors Are Part of the Contract:** Every failure mode gets a documented, machine-readable error. Clients should never parse human prose to decide what happened.
4. **Version Deliberately:** Have one explicit versioning strategy and apply it uniformly. Accidental breaking changes are the most expensive kind.
5. **Document as You Design:** The OpenAPI spec is written alongside the design, not after. If it isn't in the spec, it doesn't exist.

---

## Execution Workflow

### 1. Define Resources and Use Cases
* List the resources (nouns) and the operations clients actually need. Start from user stories, not from tables.
* Decide what is a resource vs. an action: prefer resource modeling; reserve RPC-style actions for true operations.

### 2. Design Endpoints
* Consistent naming: plural nouns, kebab-case, no verbs in paths (`POST /orders/{id}/cancel` only for genuine actions).
* Correct verbs and status codes: `GET`/`POST`/`PUT`/`PATCH`/`DELETE`; `201` with `Location` on create, `202` for accepted async work, `204` on empty success.
* Keep URLs stable: never rename or restructure without a version bump and migration path.

### 3. Specify the Error Model
* One error envelope: machine-readable `code`, human `message`, optional `details` and `request_id`.
* Map every failure to a code: validation (`400`), auth (`401`/`403`), not found (`404`), conflict (`409`), rate limit (`429`), server (`500`).
* Mark which errors are retryable so clients can implement safe retry logic.

### 4. Choose Conventions
* **Pagination:** cursor-based for large or growing datasets, offset only for small stable ones. Always return page metadata.
* **Filtering/sorting:** query params with documented operators; whitelist sortable fields.
* **Versioning:** one strategy (URL path, header, or media type) — state it and enforce it in review.

### 5. Produce the Spec
* Write the OpenAPI spec with request/response examples for the happy path and each error code.
* Include auth scheme, rate-limit headers, and idempotency-key support for mutating endpoints.

### 6. Review for Compatibility
* Check the new design against existing clients: no renamed fields, no narrowed types, no new required request fields without a version bump.

---

## Mandatory Contract Checklist

*Before finalizing the design, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Use-Case Driven:** Does every endpoint trace back to a stated client need?
2. **Consistent:** Do naming, pagination, and error shapes match existing API conventions?
3. **Error Model:** Is every failure mode documented with a machine-readable code and retry guidance?
4. **Versioned:** Is the versioning strategy explicit, and are breaking changes gated behind it?
5. **Specified:** Does the OpenAPI spec include examples for success and all error cases?
6. **Deprecation Announced:** Are deprecated endpoints, fields, or versions announced with a sunset date and a machine-readable sunset signal to clients?
7. **Migration Path:** Does every deprecated element have a documented migration path with before/after examples?

---

## Deliverables

* **🗺️ Endpoint Design:** Resources, routes, verbs, and status codes.
* **⚠️ Error Model:** The error envelope and the code catalog with retry guidance.
* **📜 OpenAPI Spec:** Complete spec with examples, auth, and rate-limit documentation.
* **🔄 Compatibility Notes:** Versioning decision and migration path for existing clients.
