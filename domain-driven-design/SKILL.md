---
name: domain-driven-design
description: >-
  Apply Vlad Khononov's Learning Domain-Driven Design principles when modeling
  domains, designing architecture, splitting services, or integrating systems.
  Use for bounded contexts, subdomains, context maps, aggregates, event sourcing,
  microservices boundaries, brownfield modernization, EventStorming, or when the
  user mentions DDD, domain-driven design, ubiquitous language, or Khononov.
---

# Domain-Driven Design — Agent Skill

Actionable rules distilled from *Learning Domain-Driven Design* (Vlad Khononov, O'Reilly 2021). **Strategy before tactics.** Match pattern to subdomain type and business complexity — not the other way around.

Pair with **clean-code** for implementation quality inside each bounded context.

## When to apply

- Designing or decomposing systems with real business complexity
- Splitting monoliths, defining service/microservice boundaries
- Modeling a domain with stakeholders (or recovering lost domain knowledge)
- Integrating legacy or third-party systems
- Choosing between transaction script, domain model, event sourcing, etc.
- User says: DDD, bounded context, subdomain, context map, aggregate, EventStorming

**Skip DDD ceremony** when business logic is trivial CRUD with no competitive value — be pragmatic.

---

## Core laws (non-negotiable)

1. **Business logic is the heart** — not frameworks, not databases, not UI.
2. **Subdomains are discovered** (problem space); **bounded contexts are designed** (solution space).
3. **Ubiquitous language** — code, tests, and conversations use the same terms domain experts use.
4. **One model per bounded context** — when models conflict, split contexts; don't force one mega-model.
5. **Heuristics, not rules** — every pattern has exceptions; validate assumptions against complexity.
6. **Start wide, decompose later** — especially for volatile core subdomains; logical boundaries before physical.
7. **Size is a poor boundary heuristic** — model drives context size, not microservice fashion.

---

## Agent workflow

```
1. STRATEGY   — identify subdomains (core / supporting / generic)
2. LANGUAGE   — name concepts with domain experts; draw context map
3. BOUNDARIES — bounded contexts + integration patterns
4. TACTICS    — decision tree → business logic + architecture + tests
5. EVOLVE     — flag when subdomain type or org changes invalidate design
6. CODE       — apply clean-code inside each context
```

For **brownfield**: understand domain → explore current design → strategic then tactical modernization. Prefer **undercover DDD** (improve without rebranding) when selling is hard.

Do **not** jump to aggregates/event sourcing for supporting/generic subdomains.

---

## Part I — Strategic design

### Subdomain types (Ch 1)

| Type | Business logic | Competitive advantage | Change rate | Invest in |
|------|----------------|----------------------|-------------|-----------|
| **Core** | Complex | Yes — differentiator | High, emergent | Best people, rich model, innovation |
| **Supporting** | Simple (ETL-like) | No — necessary but not differentiating | Low | Pragmatic, cheap, good enough |
| **Generic** | Complex but commoditized | No — buy/integrate | Medium | Off-the-shelf solutions |

**Validate type:** If you labeled it "core" but transaction script fits, revisit. Competitive advantage is not always technical.

**Identify boundaries:** Group by business capability, not org chart alone. Talk to domain experts — not only product docs.

### Ubiquitous language (Ch 2)

- Shared vocabulary between devs and domain experts
- **Consistency** — same term, same meaning everywhere in a context
- **Model** — simplified abstraction of the domain; not a copy of reality
- **Continuous effort** — refine as you learn; EventStorming and scenarios help
- Code should **speak the language** — class/method names from the domain

### Bounded contexts (Ch 3)

- **Boundary of a model** — inside: one consistent ubiquitous language
- **Not the same as subdomain** — one context can span multiple subdomains; one subdomain can appear in multiple contexts (different models)
- **Physical vs ownership boundaries** — team owns a context; don't split one context across competing teams without integration discipline
- **When models clash** (same word, different meaning — e.g. "Product" in sales vs warehouse) → separate contexts

**Sizing heuristic:** Start **wide** for uncertain/volatile core areas; include closely interacting subdomains. Refactor to narrower contexts as knowledge stabilizes.

### Context integration patterns (Ch 4)

Draw a **context map** — living artifact, not one-time diagram.

| Pattern | When |
|---------|------|
| **Partnership** | Cooperating teams; ad hoc API coordination; same org, good communication |
| **Shared kernel** | Shared integration contracts; cost of duplication > cost of coordination; **minimize shared scope**; CI on every change |
| **Customer–supplier** | Upstream/downstream; teams succeed independently |
| **Conformist** | Downstream accepts upstream model (standard, good enough, no power to negotiate) |
| **Anticorruption layer (ACL)** | Downstream protects its model; legacy/messy upstream; core subdomain downstream; frequent upstream changes |
| **Open-host service (OHS)** | Upstream publishes stable **published language** for many consumers; hide implementation model |
| **Separate ways** | Can't/won't collaborate — duplicate (especially generic, e.g. logging) or integrate separately |

**ACL vs OHS:** Consumer translates foreign model (ACL) vs supplier translates outward (OHS).

---

## Part II — Tactical design

### Business logic patterns (Ch 5–7)

| Pattern | Logic | Data | Typical subdomain |
|---------|-------|------|-------------------|
| **Transaction script** | Procedural scripts | Simple | Supporting |
| **Active record** | Methods on record objects | Complex structures | Supporting / generic integration |
| **Domain model** | Aggregates, value objects, domain services | Rich behavior | Core |
| **Event-sourced domain model** | Domain events as source of truth | Audit/history/analytics | Core + money/audit/legal needs |

**Avoid anemic domain model** — data bags with logic elsewhere when complexity warrants domain model.

### Domain model building blocks (Ch 6)

- **Value object** — identified by values; immutable; no identity (Color, Money, Address)
- **Entity** — identity matters; lifecycle
- **Aggregate** — cluster with **transaction boundary**; one **aggregate root** as public entry; reference other aggregates **by ID only**
- **Domain event** — past tense; something significant that happened; part of aggregate public interface
- **Domain service** — stateless operation that doesn't fit an entity/value object

**Aggregate rules:** Enforce invariants inside boundary; if eventually consistent data would corrupt state, keep data inside aggregate.

### Event sourcing (Ch 7)

- State = sequence of events; **source of truth** is the log
- Enables audit, temporal queries, projections
- **Requires CQRS** for practical querying (otherwise fetch-by-id only)
- Trade-offs: complexity, schema evolution, deleting data — don't default to it

### Architectural patterns (Ch 8)

| Business logic pattern | Preferred architecture |
|------------------------|------------------------|
| Transaction script | Minimal layered (3 layers) |
| Active record | Layered + application/service layer |
| Domain model | **Ports & adapters** (hexagonal) — keep aggregates persistence-ignorant |
| Event-sourced domain model | **CQRS** (obligatory) |

**CQRS note:** Commands may return data from strongly consistent model; not "commands return nothing."

**Vertical slices:** One bounded context can use different patterns per subdomain module — don't force one architecture context-wide.

### Communication patterns (Ch 9)

- **Model translation** — stateless (sync/async) or stateful when aggregating from multiple sources
- **Outbox** — reliable publish of domain events with DB transaction
- **Saga** — coordinate long-running process across aggregates/contexts (choreography)
- **Process manager** — orchestrates saga steps explicitly

**Public vs private events** — integration events ≠ internal domain events.

---

## Part III — Practice

### Tactical decision tree (Ch 10)

**Business logic (ask in order):**

1. Money/audit/deep analytics required? → **Event-sourced domain model**
2. Else complex rules/invariants/algorithms (not just CRUD validation)? → **Domain model**
3. Else complex data structures? → **Active record**
4. Else → **Transaction script**

**Complexity smell:** Language is mostly CRUD → simple; language describes processes/rules → complex.

**Architecture (from business logic choice):**

- Event-sourced → CQRS
- Domain model → Ports & adapters
- Active record → Layered (+ app layer)
- Transaction script → Minimal layered
- CQRS also when multiple persistent read models needed (any pattern)

**Testing strategy:**

| Pattern | Strategy |
|---------|----------|
| Domain model / event-sourced | **Testing pyramid** — unit tests on aggregates/VOs |
| Active record | **Testing diamond** — integration between service + persistence layers |
| Transaction script | **Reversed pyramid** — end-to-end workflow tests |

### Evolving design (Ch 11)

Subdomains **change type** over time (core→generic when commoditized, supporting→core when underestimated). Plan migrations:

- Transaction script → active record → domain model → event-sourced (each step has a path)
- Partnership → customer–supplier when teams decouple
- Growth: split wide bounded contexts; split large aggregates carefully

### EventStorming (Ch 12)

Workshop for knowledge recovery and design:

1. Unstructured exploration → 2. Timeline → 3. Pain points → 4. Pivotal events → 5. Commands → 6. Policies → 7. Read models → 8. External systems → 9. Aggregates → 10. Bounded contexts

Use for brownfield when docs are stale. Include domain experts + devs.

### Real-world DDD (Ch 13)

- **Strategic analysis first** — domain, then current design, then modernization strategy
- **Pragmatic DDD** — not all patterns everywhere
- **Selling DDD** — tie to business pain (coordination cost, legacy risk)
- **Undercover DDD** — improve boundaries and language without the label

---

## Part IV — Related methodologies

### Microservices (Ch 14)

- **Microservice ≠ bounded context** — microservice is one valid implementation of a context; context can be a module in a monolith
- Prefer **deep modules** (rich interior, small interface) over chatty shallow services
- Boundaries from **subdomains, aggregates, published language** — not "one endpoint = one service"
- Use OHS/ACL to compress public interfaces

### Event-driven architecture (Ch 15)

- Watch **distributed big ball of mud** — events everywhere without boundaries
- Coupling types: temporal, functional, implementation
- Event notification vs event-carried state transfer — choose deliberately
- Refactor toward **public/private event** separation

### Data mesh (Ch 16)

- Analytical (OLAP) model ≠ transactional (OLTP) model
- Decompose **data products by domain**; align with bounded contexts/subdomains
- DDD informs who owns which data product

---

## Smells to flag

| Smell | Action |
|-------|--------|
| One model for whole enterprise | Split bounded contexts |
| Core subdomain with transaction script | Revisit subdomain classification or invest in domain model |
| Distributed monolith | Context map; ACL/OHS; reduce cross-context chatter |
| Anemic domain model in core area | Move behavior into aggregates |
| Aggregate references another aggregate object | Reference by ID only |
| Shared kernel too large | Shrink to integration contracts |
| Microservice per table | Merge; use wide context first |
| Event sourcing "because cool" | Require audit/money/analytics justification |
| No domain experts in loop | Schedule discovery; EventStorming |
| Upstream legacy corrupting downstream | Anticorruption layer |

---

## Trade-offs

| Context | Prefer |
|---------|--------|
| Greenfield core product | Wide bounded context → domain model → decompose later |
| CRUD admin supporting feature | Transaction script or active record |
| Integrate SaaS generic capability | Conformist or local integration; don't build |
| Legacy modernization | Shared kernel (temporary) → strangler → separate contexts |
| High read/write asymmetry | CQRS projections |
| Agent-generated design | Explicit subdomain type + decision tree; don't over-pattern |

---

## Review output format

```markdown
## Domain summary
[1 sentence — what business this serves]

## Subdomain map
| Subdomain | Type | Suggested pattern |
|-----------|------|-------------------|

## Context map
[ASCII or mermaid: contexts + integration patterns]

## Must fix
- [boundary or model blockers]

## Heuristic suggestions
- [pattern choices with rationale]

## Evolution risks
- [what might change subdomain type or context boundaries]
```

Prefer diagrams and concrete names from ubiquitous language over abstract lectures.

---

## Implementation checklist (before marking done)

- [ ] Subdomain types identified (core / supporting / generic)
- [ ] Bounded contexts drawn; each has one consistent ubiquitous language
- [ ] Context map shows integration patterns (not just boxes)
- [ ] Business logic pattern matches decision tree (or exception documented)
- [ ] Architecture matches business logic pattern (ports & adapters for domain model, etc.)
- [ ] Testing strategy matches pattern (pyramid / diamond / reversed)
- [ ] Aggregates: one root, ID references across boundaries, invariants enforced
- [ ] No event sourcing/CQRS unless justified
- [ ] **clean-code** pass on code inside each context

---

## Source

Vlad Khononov, *Learning Domain-Driven Design* (O'Reilly, 2021). Personal skill for agent-assisted work.
