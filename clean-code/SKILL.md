---
name: clean-code
description: >-
  Apply Robert C. Martin's Clean Code (2nd ed.) principles when writing,
  reviewing, or refactoring code with agents at work. Use for implementation,
  PR review, refactoring, naming, tests, design, architecture, or when the user
  mentions clean code, code quality, Uncle Bob, SOLID, or Boy Scout rule.
---

# Clean Code — Agent Skill

Actionable rules distilled from *Clean Code: A Handbook of Agile Software Craftsmanship* (2nd ed., Robert C. Martin). For agent-assisted development: **make it work, then make it right** — every time.

## When to apply

- Writing or modifying production code
- Reviewing PRs or agent-generated diffs
- Refactoring, extracting, renaming
- Designing modules, APIs, or architecture
- User says: clean code, code review, SOLID, refactor, mori-code

---

## Core laws (non-negotiable)

1. **Kent Beck:** First, make it work. Then, make it right.
2. **Speed:** The only way to go fast is to go well.
3. **Boy Scout Rule:** Leave every touched file cleaner — one small improvement minimum.
4. **Read >> write:** Optimize for reading (~10:1). Code reads top-to-bottom like a story.
5. **Truth in code:** Comments lie; code is the source of truth.
6. **Professionalism:** Defend code quality like a doctor defends hand-washing.

---

## Agent workflow

```
1. DRAFT     — make it work (tests passing)
2. CLEAN     — names, extract, dedupe, remove noise comments
3. FORMAT    — team/IDE style, stepdown order
4. VERIFY    — tests green; behavior unchanged
5. BOY SCOUT — one extra micro-improvement before done
```

Do **not** big-bang rewrite unless asked. Small steps, tests green after each.

### Cleaning process (Ch 2)

1. Extract low-hanging fruit into named functions
2. Entry function = stepdown summary (calls only, reads as narrative)
3. Remove clever hacks (if it feels clever, suspect it)
4. Reduce duplication; named constants for magic numbers
5. Expand/reorganize tests — cleaning finds bugs
6. One change → verify → next

---

## First principles (Ch 3)

**Everything small, well named, organized, ordered.**

| Pillar | Rule |
|--------|------|
| Small | Handful of lines per function; one thing |
| Well named | For the *next reader*, not your current understanding |
| Organized | Business rules live where they belong |
| Ordered | Caller above callee; stepdown narrative |

**Before growth:** Tidy accreting switches before the next feature wave.

**SOLID (apply when domain will grow):**

- **SRP** — one reason to change per module
- **OCP** — extend via new types, not editing core switches
- **LSP** — subtypes honor the contract
- **ISP** — don't depend on unused interfaces
- **DIP** — high-level policy → abstractions; details are plug-ins

**Mantra:** High-level policy must not depend on low-level details.

**YAGNI:** Ask *"What if we aren't going to need it?"* — count cost; invest when growth is real.

---

## Naming (Ch 4 — Tim Ottinger)

Write for **maintainers**. Rename freely.

**Do:** intention-revealing, pronounceable, searchable, meaningful distinctions, one word per concept, nouns for classes, verbs for methods, meaningful context (`Address` not bare `state`).

**Length:** variables longer in wider scope; global functions/classes shorter; private helpers and tests longer.

**Avoid:** disinformation (`accountList` when not a List), encodings (Hungarian, `m_`, `I` prefix), noise (`Data`, `Info`, `Manager`), cute names, gratuitous prefixes (`GSDAccountAddress`).

**Domain:** problem domain for business; solution domain for algorithms/patterns (`Visitor`, `Queue`).

---

## Comments (Ch 5)

**Default:** Prefer code over comments. Need a comment? Try extract function/variable first.

**Keep:** legal headers, public API docs, regex/format hints, intent/warnings, amplification of easy-to-skip critical detail.

**Delete:** redundant, mumbling, misleading, mandated Javadoc on obvious code, journals, checked-in TODO, commented-out code, attributions, banner markers, nonlocal info.

**Never:** comment bad code instead of cleaning it.

---

## Formatting (Ch 6)

Communication via layout. **One team formatter** — follow it.

**Vertical:** blank line between concepts; no blank between related fields; files ~200 typical / ~500 max; caller above callee; group similar functions.

**Horizontal:** ≤120 chars; space around `=`; no space before `(` on calls; space after `,`; no column-alignment of declarations.

**Indent:** preserve scope hierarchy; expand short if/while blocks.

---

## Functions (Ch 7–11)

- **Small** — ideally 2–4 lines; blocks in if/while often one function call
- **One level of abstraction** per function — no mixing policy and details
- **Stepdown rule** — read top-to-bottom; each function followed by callees
- **Do one thing** — extract till drop when unsure
- **Few arguments** — prefer ≤3; prefer none when context prepared
- **No output args** — prefer return values; avoid mutating inputs
- **Command Query Separation** — function commands OR queries, not both
- **Prefer exceptions** to error codes (avoid dependency magnets)
- **Switch statements** — isolate behind polymorphism when they grow
- **DRY** — eliminate real duplication (same reason to change); allow accidental duplication (different reasons)
- **Side effects:** minimize; make obvious; beware temporal coupling
- **Structured programming:** sequence, selection, iteration — no goto logic
- **Be polite:** newspaper metaphor — high summary first, details below
- **Descriptive names** over comments for control flow: `if (employee.isEligibleForFullBenefits())`

Functions are **verbs** of the domain language; contexts are **nouns**.

---

## Objects & classes (Ch 12–13)

- **Data abstraction** over data structures when behavior matters
- **Law of Demeter:** don't talk to strangers — `a.getB().getC().doX()` is a smell
- **DTOs** for pure data transfer; don't smuggle behavior
- **Classes:** one responsibility; small; instance vars in one known place (top in Java/C#)
- **Extract classes** when declaration lists get long (alignment is a smell)

---

## Tests (Ch 14–16)

**Disciplines (pick one, honor it):**

- **TDD:** (1) failing test first (2) minimal code to pass (3) refactor with tests green
- **TCR:** test && commit || revert
- **Small bundles:** small code+test batches, full coverage before moving on

**F.I.R.S.T.:**

- **Fast** — suite runs quickly
- **Independent** — no order dependency
- **Repeatable** — any environment
- **Self-validating** — pass/fail, no manual inspection
- **Timely** — written with (or just before) production code

**Clean tests:** domain-specific testing language; decouple tests from production details; keep tests as clean as production code.

**Acceptance tests:** formal collaboration language between business and dev.

**With AI:** ~20% of generated code may be defective — **comprehensive behavioral tests are mandatory**. Regenerate at module level when design is SRP-compliant; fix at small class/method granularity.

---

## Simple design (Ch 18 — Kent Beck)

Priority order:

1. **Covered by tests**
2. **Reveals intent** (code + tests together express usage)
3. **Minimizes duplication** (real duplication only)
4. **Minimizes size** (fewest elements without hurting above)

---

## Components & continuous design (Ch 20–21)

- **REP, CCP, CRP** — reuse, develop together, deploy together (balance the triangle)
- **ADP, SDP, SAP** — acyclic dependencies; depend in direction of stability
- **Continuous design:** every choice affects future cost; refactor in small verified steps
- **Four Cs:** cohesion, conciseness, clarity, confirmability — extract functions liberally

---

## Concurrency (Ch 22)

- Keep concurrent code **small and isolated**
- Limit shared mutable state
- Prefer immutability and functional style where practical
- Defensive principles: single responsibility for threads, data ownership, synchronized regions

---

## Architecture (Ch 23–27)

**Two values:** behavior + **structure** (structure enables change — greater long-term value).

**Policy vs detail:** architecture makes details pluggable and decisions deferrable (DB, UI, frameworks).

**Independence dimensions:** use cases, operation, development (Conway), deployment.

**Dependency Rule:** source dependencies point **inward** toward policy. DB and UI are plug-ins.

**Layers (Clean Architecture):** Entities → Use Cases → Interface Adapters → Frameworks/Drivers.

**Boundaries:** few places touch third-party code; use adapters; **learning/boundary tests** for external APIs.

**Hexagonal / Ports & Adapters:** inside = domain logic; outside = IO, DB, UI.

---

## AI & LLMs (Ch 17)

- AI raises abstraction; **code never disappears** — prompts/specs must be precise
- **Prompt quality matters** — ambiguous prompts → ambiguous programs
- Prefer **incremental changes** over full regeneration (regeneration breaks working behavior)
- Require **overloaded formal specs** (definitions + tests/scenarios) — single precise statement is not enough
- LLMs: good at "make it work" drafts; **bad at architectural depth** (dependency inversion, policy/detail split) without explicit instruction
- Instruct AI: small SRP modules, functional style where fit, clarity over cleverness, tests from examples

---

## Craftsmanship (Ch 28–37) — for agent behavior

- **Do no harm** — to society, behavior, or structure
- **No defect** in behavior or structure when shipping
- **Repeatable proof** — tests, TDD, structured programming
- **Small cycles** — CI, frequent integration, short feedback loops
- **Relentless improvement** — coverage, mutation testing where valuable, continuous cleaning
- **Honest estimates** — don't lie about time; separate accuracy vs precision
- **Respect** fellow programmers — readable diffs, no gratuitous churn
- **Never stop learning** — update skill as practices evolve

---

## Smells to flag

| Smell | Action |
|-------|--------|
| Giant function/class | Extract, split |
| Mixed abstraction levels | Extract, stepdown |
| Long arg lists | parameter object / context |
| Duplicated logic | Extract; watch accidental vs real |
| Clever one-liner | Expand for clarity |
| Comment wall | Replace with names |
| Commented-out code | Delete |
| Checked-in TODO | Backlog or fix |
| Feature envy / train wrecks | Law of Demeter, move method |
| Switch on type growing | Polymorphism / strategy |
| God class / long file | Split by responsibility |
| Dependency cycles | DIP, interfaces |
| AI code without tests | Block until tested |

---

## Trade-offs

| Context | Prefer |
|---------|--------|
| Business/app code | Clarity over micro-optimization |
| Hot path / embedded | Measure; clarity within budget |
| Team codebase | Team conventions + formatter |
| Agent draft | Always "make it right" pass |
| Premature abstraction | YAGNI until second use proves need |

---

## Review output format

```markdown
## Summary
[1 sentence]

## Must fix
- [behavior or readability blockers]

## Boy Scout suggestions
- [small cleanups]

## Smells
- [named smell → suggested fix]
```

Prefer a small cleaned snippet over lectures. Minimal, actionable.

---

## Implementation checklist (before marking done)

- [ ] Tests pass; behavior unchanged
- [ ] Names reveal intent (no `d`, `x`, `data` in wide scope)
- [ ] Functions small, one abstraction level, stepdown order
- [ ] No noise/redundant comments; no commented-out code
- [ ] Formatted per project style
- [ ] Boy Scout improvement applied
- [ ] No new duplication without abstraction
- [ ] Dependencies point toward policy (if architectural change)

---

## Source

Robert C. Martin, *Clean Code* 2nd Edition (2025). Personal skill for agent-assisted work.
