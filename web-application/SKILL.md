---
name: web-application
description: >-
  Design full-stack web applications using combined principles from web usability
  (Krug), web API design (Lauret), data-intensive backends (Kleppmann DDIA), and
  software architecture (Richards & Ford). Use for end-to-end web app design,
  full-stack reviews, or when the user mentions web application, full stack,
  frontend and backend together.
---

# Web Application — Unified Agent Skill

End-to-end web app design by combining four book skills. **User-facing clarity + consumer-first APIs + data that scales + architecture trade-offs.**

Use sub-skills for depth; this skill **orchestrates** them.

| Layer | Skill | Book |
|-------|-------|------|
| UX / UI | **web-usability** | Don't Make Me Think (Krug) |
| HTTP API | **web-api-design** | The Design of Web APIs (Lauret) |
| Data / backend storage | **ddia** | Designing Data-Intensive Applications (Kleppmann) |
| System shape / teams | **software-architecture** | Fundamentals of Software Architecture (Richards & Ford) |

Also pair: **clean-code**, **domain-driven-design**, **design-patterns** from the core track.

---

## When to apply

- Designing a new web application (frontend + backend + data)
- Full-stack architecture review
- Connecting UI flows to API and data choices
- User says: web app, full stack, build this SaaS, end-to-end design

---

## Core laws (combined)

1. **Users don't read; developers don't read your mind** — UX for humans, API for consumers.
2. **Hide internals at every boundary** — UI doesn't expose DB; API doesn't leak tables.
3. **Data outlives code** — choose storage and consistency deliberately (DDIA).
4. **Architecture is trade-offs** — style follows characteristics, not hype (FSA).
5. **Simplest system that meets characteristics** — modular monolith + Postgres often enough.
6. **Test UX cheaply; version APIs carefully.**

---

## Full-stack agent workflow

```
1. USERS & JOBS     → web-usability: tasks, scan design, flows
2. DOMAIN & BOUNDS  → domain-driven-design: contexts (if complex)
3. ARCHITECTURE     → software-architecture: characteristics, style, components
4. API CONTRACT     → web-api-design: resources, OAS, errors
5. DATA LAYER       → ddia: model, replication, cache, streams
6. IMPLEMENTATION   → clean-code + design-patterns
7. VERIFY           → 3-user UX test plan + API review + data failure modes
```

### Layer routing — which skill leads?

| Task focus | Lead skill |
|------------|------------|
| Page layout, nav, copy, forms | web-usability |
| REST endpoints, OpenAPI, errors | web-api-design |
| Database, cache, queues, consistency | ddia |
| Monolith vs services, ADRs, modularity | software-architecture |
| Whole feature / greenfield app | **web-application** (this skill) |

---

## Typical web app stack (default heuristic)

**Small/medium product (default):**
- **Frontend:** SPA or server-rendered; Krug rules on every screen
- **API:** REST + OpenAPI; consumer-first resources
- **Data:** PostgreSQL OLTP; Redis cache if read-heavy
- **Style:** Modular monolith or service-based (1–3 deployables)
- **Search/analytics:** derived later via CDC/batch — not day one

**Scale triggers → escalate:**
- Team boundaries + independent deploy → software-architecture (microservices?)
- Read lag / global users → ddia (replication, CDN, cache)
- Partner integrations → web-api-design (versioning, auth)
- Conversion/trust issues → web-usability (test, simplify)

---

## Cross-layer checks

Before shipping a feature, verify:

| Check | Skill |
|-------|-------|
| 5-second test on main screens | web-usability |
| One error format across API | web-api-design |
| No N+1 API calls for one screen | web-api-design |
| Migrations backward compatible | ddia |
| Backups + replication on prod data | ddia |
| ADR if new service or DB | software-architecture |
| Auth at API boundary | web-api-design |
| Loading/error states in UI | web-usability |

---

## Full-stack review output format

```markdown
## User jobs
[What users need — web-usability]

## Architecture
[Style, components, characteristics — software-architecture]

## API surface
[Key resources, flows — web-api-design]

## Data
[Storage, consistency, cache — ddia]

## Cross-layer issues
[UI↔API mismatches, chatty APIs, missing error UX]

## Recommended next steps
[Ordered fixes]
```

---

## Smells (full-stack)

| Smell | Layer | Fix |
|-------|-------|-----|
| UI mirrors DB tables | API + UX | Resource model + user mental model |
| 10 API calls per page | API | Aggregate endpoints / expand |
| No loading states | UX | Feedback on async |
| Microservices day one | Arch | Modular monolith first |
| Shared DB across services | Arch + Data | Partition or explicit shared kernel |
| Breaking API without version | API | Evolution policy |
| No usability test | UX | Monthly 3-user test |

---

## Invocation examples

```
Design this web app with web-application
```

```
Full-stack review — web-application + clean-code
```

```
Just the API layer — web-api-design
```

```
Just database choice — ddia
```

---

## Source skills

- `~/.cursor/skills/web-usability/SKILL.md`
- `~/.cursor/skills/web-api-design/SKILL.md`
- `~/.cursor/skills/ddia/SKILL.md`
- `~/.cursor/skills/software-architecture/SKILL.md`

Books in `Learning/software-development/web-application/materials/`.
