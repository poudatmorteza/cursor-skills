---
name: software-architecture
description: >-
  Apply Fundamentals of Software Architecture (Richards & Ford) when choosing
  architecture styles, defining components, trade-offs, ADRs, modularity, or
  team structure. Use for monolith vs microservices, layered/event-driven
  styles, architecture characteristics, or when the user mentions software
  architect, architecture decision, or Richards/Ford.
---

# Software Architecture — Agent Skill

Rules from *Fundamentals of Software Architecture* (Mark Richards & Neal Ford, O'Reilly 2020). **Architecture is trade-offs — "it depends" is the honest answer.**

Pair with **domain-driven-design** for boundaries; **ddia** for data; **design-patterns** for code structure.

## When to apply

- Choosing architecture style (monolith, microservices, event-driven, etc.)
- Identifying architecture characteristics (performance, elasticity, deployability)
- Component boundaries, modularity, coupling metrics
- Writing Architecture Decision Records (ADRs)
- User says: architecture, monolith, microservices, ADR, trade-off, architect

---

## Core laws

1. **Everything in architecture is a trade-off.** (First Law)
2. **Why is more important than how.** (Second Law)
3. **Architecture ≠ design** — architecture = what's hard to change; design = day-to-day structure.
4. **Characteristics drive style** — pick style from required -ilities, not fashion.
5. **Least worst architecture** — no perfect; document what you sacrifice.
6. **Architects decide; teams implement** — decisions need compliance and clarity.

---

## Agent workflow

```
1. DRIVERS     — business domain + requirements → architecture characteristics
2. MEASURE     — rate characteristics (1–5) for candidate styles
3. COMPONENTS  — identify boundaries, granularity, quanta
4. STYLE       — choose least-worst style(s) for context
5. DECIDE      — ADR for significant choices
6. GOVERN      — fitness functions / reviews keep architecture from drifting
```

Answer **"it depends"** with **what it depends on**.

---

## Architect role (Ch 1)

Must: make architecture decisions, analyze continuously, stay current, ensure compliance, domain knowledge, interpersonal skills, navigate politics.

Balance: **hands-on coding** vs architecture work — stay technical enough to earn trust.

---

## Architectural thinking (Ch 2)

- **Technical breadth** over depth in one stack — T-shaped
- **Trade-off analysis** — every recommendation lists pros/cons
- **Business drivers** — architecture serves business, not resume
- Anti-pattern: **Frozen Caveman** — one past disaster drives all decisions

---

## Modularity (Ch 3)

| Metric | Meaning |
|--------|---------|
| **Cohesion** | Things that change together live together |
| **Coupling** | Minimize dependencies between modules |
| **Abstractness / Instability** | Main sequence distance — balanced modules |
| **Connascence** | Coupling type (name, type, timing, etc.) |

**Modules → components** — deployable units with explicit contracts.

---

## Architecture characteristics (Ch 4–6)

**Operational:** performance, scalability, elasticity, availability, fault tolerance…

**Structural:** modularity, deployability, testability, maintainability…

**Cross-cutting:** security, accessibility, usability (for user-facing systems)…

Extract from:
- **Domain** (e.g. auction → high elasticity, low latency)
- **Requirements** (explicit + implicit)

**Measure & govern:** fitness functions, tests, reviews — architecture rots without them.

---

## Component thinking (Ch 8)

1. Identify initial components from workflows/domains
2. Assign requirements to components
3. Analyze roles and architecture characteristics
4. Restructure for **right granularity**

**Architecture quantum:** independently deployable unit including required data (may include DB).

**Monolith vs distributed:** distributed adds network fallacies — don't distribute without cause.

---

## Distributed fallacies (Ch 9)

1. Network is reliable  
2. Latency is zero  
3. Bandwidth is infinite  
4. Network is secure  
5. Topology doesn't change  
6. One administrator  
7. Transport cost is zero  
8. Network is homogeneous  

**Default:** prefer modular monolith until characteristics force distribution.

---

## Architecture styles (Part II) — selection summary

| Style | Good for | Watch out |
|-------|----------|-----------|
| **Layered** | Simple CRUD, small teams | Architecture sinkhole, coupling layers |
| **Pipeline** | Sequential processing | Error handling across filters |
| **Microkernel** | Product + plugins | Plugin contract stability |
| **Service-based** | Few coarse services | Still mostly monolith DB |
| **Event-driven** | Reactivity, decoupling | Debugging, eventual consistency |
| **Microservices** | Independent deploy, scale teams | Distributed complexity, data |
| **SOA (orchestration)** | Enterprise integration | Heavy ESB, central bottleneck |
| **Space-based** | Extreme elasticity | Complexity, cost |

**Ch 18:** Choose by **characteristics ratings**, not hype. Modular monolith often beats microservices early.

---

## Techniques (Part III)

### ADRs (Ch 19)
- Document **architecturally significant** decisions
- Status, context, decision, consequences
- Avoid: Covering Your Assets, Groundhog Day, Email-Driven Architecture

### Risk (Ch 20)
- **Risk storming** with teams before build

### Diagrams (Ch 21)
- C4 or simple boxes — **for communication**, not decoration
- Incremental reveal in presentations

### Teams (Ch 22–23)
- **Conway's law** — structure follows communication
- Architect as **pragmatic visionary** — enable teams, don't dictate from armchair
- **4 C's:** communication, collaboration, clarity, cleanliness

---

## Smells to flag

| Smell | Fix |
|-------|-----|
| Microservices because Netflix | Match characteristics to style |
| No documented decisions | ADRs |
| Shared database across services | Database per service or explicit shared kernel |
| Architect never codes | Hands-on credibility |
| One-size-fits-all style | Style per quantum/component |
| Ignoring operability | DevOps/observability in characteristics |
| Fashion-driven rewrite | Trade-off analysis |

---

## Review output format

```markdown
## Business drivers
[Domain, requirements]

## Architecture characteristics
[Top 5, rated]

## Recommended style
[Choice + trade-offs]

## Components / boundaries
[Major parts, data ownership]

## ADR candidates
[Decisions needing records]

## Risks
[Technical, organizational]
```

---

## Source

Mark Richards & Neal Ford, *Fundamentals of Software Architecture* (O'Reilly, 2020).
