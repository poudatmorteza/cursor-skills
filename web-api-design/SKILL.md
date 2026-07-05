---
name: web-api-design
description: >-
  Apply Arnaud Lauret's The Design of Web APIs principles when designing,
  reviewing, or documenting REST/OpenAPI APIs. Use for resources, HTTP methods,
  error models, versioning, security, consumer-first API design, or when the
  user mentions REST API, OpenAPI, API design, or Lauret.
---

# Web API Design — Agent Skill

Rules from *The Design of Web APIs* (Arnaud Lauret, Manning 2019). **APIs are software's UI for developers — design for consumers, not your database.**

Pair with **domain-driven-design** for bounded contexts; **ddia** for data/storage; **clean-code** for handler code.

## When to apply

- Designing REST (or REST-like) HTTP APIs
- Reviewing OpenAPI specs, endpoints, error responses
- API versioning, security, documentation
- User says: REST API, OpenAPI, API design, endpoints, web API

---

## Core laws

1. **Consumer-first** — design what callers need to do, not how your DB is shaped.
2. **Hide implementation** — API is a contract; internals can change behind it.
3. **Straightforward** — clear names, types, data; no puzzles.
4. **Predictable** — consistent patterns across the whole API.
5. **Concise & organized** — minimal surface; logical grouping.
6. **Secure by design** — auth, scopes, input validation, least exposure.
7. **Evolvable** — version and extend without breaking consumers.

---

## Agent workflow

```
1. GOALS CANVAS — whats (user goals), inputs/outputs, all consumer types
2. RESOURCES    — nouns, relationships, not DB tables
3. ACTIONS      — HTTP methods on resources; idempotency where needed
4. DATA         — concepts, responses, parameters (JSON Schema / OAS)
5. USABILITY    — straightforward → predictable → concise
6. CONTEXT      — security, evolution, network efficiency, docs
```

**Reject provider perspective:** don't expose internal IDs, org structure, or ORM shapes without translation.

---

## API goals canvas (Part 1)

For each capability ask:
- **What** can consumers accomplish? (not how server works)
- **Inputs / outputs** — data in, data out
- **All users** — mobile app, partner, internal admin, future consumers
- **Missing goals** — what consumers need but you haven't listed

**Anti-patterns to catch:**
- Data model leaked as API (`/users/123/orders/456/line_items/789`)
- Business logic steps exposed as chained calls consumers shouldn't orchestrate
- Team boundaries visible in endpoint chaos

---

## REST mapping (Ch 3)

| Concept | REST expression |
|---------|-----------------|
| Resource | Noun in path (`/payments`, `/customers/{id}`) |
| Collection | Plural path; create via POST to collection |
| Relationship | Sub-resource or link (`/customers/{id}/orders`) |
| Action on resource | HTTP method + path (prefer standard CRUD) |
| Non-CRUD action | POST to sub-path (`/orders/{id}/cancel`) sparingly |

**HTTP cheat sheet:**
- GET — read, safe, idempotent
- POST — create / non-idempotent action
- PUT — full replace, idempotent
- PATCH — partial update
- DELETE — remove, idempotent

Use **standard status codes** with consistent error body shape.

---

## Data design (Ch 3–4)

- **Concepts** — stable domain names in JSON (not `fld_usr_nm`)
- **Responses** — ready-to-use; avoid N+1 client assembly when reasonable
- **Parameters** — clear types/formats (dates, money, enums)
- **OpenAPI (OAS)** — single source for docs, codegen, review
- **Reuse components** — `$ref` schemas; don't duplicate

---

## Part 2 — Usable API design

### Straightforward (Ch 5)
- Crystal-clear names (`createdAt` not `ts`)
- Easy types (ISO-8601 dates, string enums with known values)
- **Ready-to-use data** in responses — don't make clients join 5 calls
- **Exhaustive errors** — every failure mode documented and returned consistently
- Informative success feedback (201 + Location, body with id)

### Predictable (Ch 6)
- **Consistency** — same naming, pagination, error format everywhere
- Same concept = same name across endpoints
- Pagination/filter/sort patterns repeated

### Concise & organized (Ch 7)
- Don't expose internal/admin fields on public API
- Aggregate goals when consumers always need combined data
- **Stateless flows** — each request self-contained where possible

---

## Part 3 — Contextual design

### Security (Ch 8)
- Authn/authz on every sensitive operation
- Scopes/roles explicit in design
- Never trust client input; validate at boundary
- Don't leak stack traces or internal IDs in errors

### Evolution (Ch 9)
- **Backward compatible** changes preferred (add optional fields)
- Breaking changes → new version or explicit migration path
- Deprecation headers + timeline
- Design for unknown future consumers

### Network efficiency (Ch 10)
- Pagination, field selection (`?fields=`), compression
- Avoid chatty APIs when batch endpoints are justified
- Caching headers where reads are cacheable

### Documentation (Ch 12–13)
- OAS complete: examples, error cases, auth
- **Review API designs** before build — API design is product design
- Grow APIs deliberately; resist endpoint sprawl

---

## Smells to flag

| Smell | Fix |
|-------|-----|
| RPC verbs in paths (`/getUser`, `/doPayment`) | Resources + HTTP methods |
| DB tables as endpoints | Resource model from consumer goals |
| Inconsistent error shapes | One error schema |
| 200 with error in body | Proper 4xx/5xx |
| Missing pagination on lists | Cursor/offset pattern |
| Breaking changes without version | Version or compat policy |
| Leaking stack traces | Safe error messages |
| Consumer needs 10 calls for one screen | Aggregate or expand |

---

## Review output format

```markdown
## Consumer goals
[What callers need to accomplish]

## Resource model
[Paths, methods, relationships]

## Data & errors
[Schemas, status codes, error consistency]

## Usability
[Straightforward / predictable / concise issues]

## Security & evolution
[Auth, breaking change risks]

## OpenAPI gaps
[Missing docs, examples, components]
```

---

## Source

Arnaud Lauret, *The Design of Web APIs* (Manning, 2019).
