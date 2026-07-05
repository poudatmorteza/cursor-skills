---
name: design-patterns
description: >-
  Apply Gang of Four Design Patterns (Gamma, Helm, Johnson, Vlissides) when
  designing, implementing, or refactoring object-oriented code. Use for pattern
  selection, creational/structural/behavioral design, flexibility without
  over-engineering, or when the user mentions design patterns, GoF, Strategy,
  Factory, Observer, Decorator, or Lexi case study.
---

# Design Patterns (GoF) — Agent Skill

Actionable rules distilled from *Design Patterns: Elements of Reusable Object-Oriented Software* (Gamma, Helm, Johnson, Vlissides, 1994). Patterns capture **proven designs** — not clever tricks. Apply only when the flexibility is **actually needed**.

Pair with **clean-code** for implementation quality; **domain-driven-design** when boundaries and domain models matter.

## When to apply

- Choosing how to structure classes/objects for change
- Refactoring toward loose coupling, composability, or pluggable behavior
- Integrating incompatible interfaces, hiding platform details, or extending without subclass explosion
- User says: design pattern, GoF, Strategy, Factory, Observer, Decorator, Adapter

**Do not** sprinkle patterns for resume-driven design. Read **Consequences** before applying.

---

## Core laws (non-negotiable)

1. **Program to an interface, not an implementation** — clients depend on abstractions.
2. **Favor object composition over class inheritance** — compose behavior; don't subclass for every variant.
3. **Encapsulate what varies** — each pattern isolates one aspect that changes independently.
4. **Delegation over inheritance** — Strategy, State, Visitor delegate to helper objects.
5. **Run-time structure ≠ compile-time structure** — many patterns build dynamic object graphs (Composite, Decorator, Observer).
6. **Patterns are not frameworks** — you implement them each time; they document intent and trade-offs.
7. **Only apply when needed** — extra indirection has cost (complexity, performance).

---

## Agent workflow

```
1. PROBLEM    — name the design force (what must vary? what causes redesign?)
2. SELECT     — match cause of redesign OR aspect to vary (Table below)
3. VALIDATE   — check Applicability + Consequences; consider simpler fix first
4. NAME       — use domain names + pattern role (e.g. TeXLayoutStrategy)
5. IMPLEMENT  — minimal structure; prefer composition
6. VERIFY     — clean-code pass; don't over-abstract
```

**Selection order:** (1) cause of redesign → (2) intent scan → (3) related patterns graph → (4) compare patterns of same purpose.

---

## Two OOD principles (Ch 1)

| Principle | Rule |
|-----------|------|
| **Program to interface** | Declare variables, parameters, returns, and attributes as abstract types (interfaces/base classes), not concrete classes. |
| **Favor composition** | Reuse by assembling objects; inheritance for true is-a specialization only. Use delegation in Strategy, State, Bridge. |

---

## Causes of redesign → patterns (Ch 1.6)

| Cause | Patterns |
|-------|----------|
| Creating objects by explicit class name | Abstract Factory, Factory Method, Prototype |
| Dependence on specific operations | Chain of Responsibility, Command |
| Platform (OS/API) dependence | Abstract Factory, Bridge |
| Dependence on representation/location/implementation | Abstract Factory, Bridge, Memento, Proxy |
| Algorithmic dependencies | Builder, Iterator, Strategy, Template Method, Visitor |
| Tight coupling | Abstract Factory, Bridge, Chain of Responsibility, Command, Facade, Mediator, Observer |
| Extending by subclassing only | Bridge, Chain of Responsibility, Composite, Decorator, Observer, Strategy |
| Can't modify class conveniently | Adapter, Decorator, Visitor |

Ask: **"What should be variable without redesign?"** — pick pattern that encapsulates that aspect.

---

## Pattern catalog — intents (23 patterns)

### Creational — object creation

| Pattern | Intent |
|---------|--------|
| **Abstract Factory** | Interface for creating *families* of related objects without specifying concrete classes |
| **Builder** | Separate construction of complex object from representation; same process → different representations |
| **Factory Method** | Interface for creating an object; *subclasses* decide which class to instantiate |
| **Prototype** | Create objects by copying a prototypical instance |
| **Singleton** | Ensure one instance + global access (use sparingly — testability cost) |

**Choose creational:** Factory Method = subclass parameterization. Abstract Factory / Builder / Prototype = object-composition parameterization (factory object). Prototype when classes vary at runtime or cloning is costly to subclass.

### Structural — composition & interfaces

| Pattern | Intent |
|---------|--------|
| **Adapter** | Convert interface so incompatible classes work together (*after* design — unforeseen coupling) |
| **Bridge** | Decouple abstraction from implementation so both vary independently (*before* — known multiple impls) |
| **Composite** | Tree structures; treat individual and composite objects uniformly |
| **Decorator** | Attach responsibilities dynamically; alternative to subclassing for extension |
| **Facade** | Unified interface to a subsystem — *new* simplified interface |
| **Flyweight** | Share fine-grained objects efficiently via extrinsic state |
| **Proxy** | Surrogate controlling access (lazy load, protection, remote, caching) |

**Adapter vs Bridge:** Adapter fixes two existing interfaces; Bridge plans multiple implementations upfront. **Adapter vs Facade:** Adapter reuses old interface; Facade defines new higher-level one. **Composite vs Decorator vs Proxy:** Similar structure; intent differs — part-whole hierarchy vs add behavior vs control access.

### Behavioral — algorithms & responsibility

| Pattern | Intent |
|---------|--------|
| **Chain of Responsibility** | Pass request along chain until something handles it |
| **Command** | Encapsulate request as object — parameterize, queue, log, **undo** |
| **Interpreter** | Grammar + interpreter for a small language |
| **Iterator** | Sequential access to aggregate without exposing representation |
| **Mediator** | Encapsulate how a set of objects interact — reduces explicit references |
| **Memento** | Capture/restore internal state without breaking encapsulation |
| **Observer** | One-to-many dependency; dependents notified on state change |
| **State** | Alter behavior when internal state changes — appears to change class |
| **Strategy** | Family of interchangeable algorithms encapsulated |
| **Template Method** | Algorithm skeleton in base class; subclasses override steps |
| **Visitor** | New operation on elements without changing their classes |

**Encapsulating variation:** Strategy (algorithm), State (state-dependent behavior), Mediator (interaction protocol), Iterator (traversal). Chain of Responsibility = dynamic open-ended chain.

---

## Aspects you can vary (selection table)

| Pattern | Vary independently |
|---------|-------------------|
| Abstract Factory | Families of product objects |
| Builder | How composite object is created |
| Factory Method | Subclass of object instantiated |
| Prototype | Class of object instantiated |
| Singleton | Sole instance of a class |
| Adapter | Interface to an object |
| Bridge | Implementation of an object |
| Composite | Structure/composition of an object |
| Decorator | Responsibilities without subclassing |
| Facade | Interface to subsystem |
| Flyweight | Storage cost of objects |
| Proxy | How/where object is accessed |
| Chain of Responsibility | Object that fulfills request |
| Command | When/how request is fulfilled |
| Interpreter | Grammar and interpretation |
| Iterator | How aggregate elements are accessed |
| Mediator | How/which objects interact |
| Memento | What private state is externalized |
| Observer | Dependents and how they stay updated |
| State | States of an object |
| Strategy | Algorithm |
| Template Method | Steps of an algorithm |
| Visitor | Operations on object structure |

---

## MVC mapping (canonical example)

Smalltalk MVC demonstrates three patterns working together:

| MVC piece | Pattern |
|-----------|---------|
| Model notifies Views | **Observer** |
| Nested views | **Composite** |
| Controller response strategy | **Strategy** |
| Default controller / scrolling | Factory Method, **Decorator** |

Use this when explaining publish-subscribe UI or pluggable behavior.

---

## Lexi case study — pattern → problem (Ch 2)

| Problem | Pattern(s) |
|---------|------------|
| Document tree structure | **Composite** (recursive composition) |
| Multiple formatting policies | **Strategy** |
| Embellish UI (scrollbars, borders) | **Decorator** |
| Multiple look-and-feel | **Abstract Factory** |
| Multiple window systems | **Bridge** |
| User operations + undo | **Command** |
| Spelling / hyphenation on structure | **Visitor** |

---

## Pattern comparisons (chapter discussions)

### Creational
- **Factory Method** — easiest start; subclass proliferation risk.
- **Abstract Factory** — product *families*; large factory hierarchies.
- **Prototype** — clone existing instances; hide concrete classes from clients.
- **Builder** — step-by-step complex assembly; separate director if needed.
- **Singleton** — global access; prefer DI in modern code when possible.

### Structural
- **Adapter** — retrofit compatibility. **Bridge** — intentional abstraction/implementation split.
- **Decorator** — transparent wrapper adding behavior. **Proxy** — access control, not feature stacking.
- **Flyweight** — many similar objects; extrinsic state passed in. Identity tests may lie.

### Behavioral
- **Strategy vs Template Method** — composition/delegation vs inheritance; Strategy varies algorithm at runtime.
- **State vs Strategy** — State transitions are internal; Strategy chosen by client/context.
- **Command vs Strategy** — Command encapsulates full request + undo; Strategy is algorithm slot.
- **Observer vs Mediator** — Observer broadcast; Mediator centralizes interaction rules.
- **Visitor vs double dispatch** — add operations without changing element classes; new element types hurt Visitor.

---

## Modern usage notes (agent)

| Pattern | Modern note |
|---------|-------------|
| Singleton | Prefer explicit scope/DI; test doubles suffer |
| Abstract Factory | Often replaced by DI containers + interfaces |
| Iterator | Built into language (`for`, generators) — still apply concept |
| Observer | Events, pub/sub, reactive streams — same force |
| Decorator | Middleware, wrappers, aspect-like stacking |
| Command | Undo stacks, job queues, CQRS commands |
| Adapter | Anti-corruption / API clients (see domain-driven-design) |
| Facade | Module boundaries, API gateways |
| Template Method | Hooks vs callbacks; prefer Strategy when runtime swap needed |
| Visitor | AST walks; switch on type smell alternative when types stable |

Functional languages: some patterns are language features (e.g. first-class functions ≈ Strategy). Still name the **force** being addressed.

---

## Smells → pattern direction

| Smell | Consider |
|-------|----------|
| `new ConcreteClass()` everywhere | Factory Method, Abstract Factory, Prototype |
| Giant switch on type | State, Strategy, Visitor |
| Subclass explosion for variants | Strategy, Decorator, Bridge |
| Two incompatible APIs | Adapter |
| Client knows too much of subsystem | Facade, Mediator |
| Subclass only to change one algorithm | Strategy |
| Undo/redo missing | Command + Memento |
| UI/platform `#ifdef` mess | Abstract Factory, Bridge |
| Million tiny identical objects | Flyweight |
| Lazy load / access control | Proxy |
| One object notifies many | Observer |
| Complex object construction | Builder |

**First ask:** Can a simpler refactor (extract function, inject dependency) suffice? **YAGNI** applies to patterns.

---

## Review output format

```markdown
## Design force
[What must vary or what redesign risk]

## Recommended pattern(s)
- [Name] — [one-line intent match]

## Alternatives considered
- [Pattern] — why not

## Consequences
- [Flexibility gained vs complexity/cost]

## Implementation sketch
[Domain-named roles, composition over inheritance]
```

---

## Implementation checklist (before marking done)

- [ ] Pattern addresses a **real** cause of redesign or variation point
- [ ] Clients depend on **abstractions**, not concrete classes
- [ ] Composition/delegation preferred over deep inheritance
- [ ] Participant names reflect **domain** (Strategy suffix where helpful)
- [ ] Consequences accepted (indirection, object count, identity)
- [ ] Not over-applied — simpler design rejected for good reason
- [ ] **clean-code** pass on resulting structure

---

## Source

Gamma, Helm, Johnson, Vlissides — *Design Patterns* (Addison-Wesley, 1994). Personal skill for agent-assisted work.
