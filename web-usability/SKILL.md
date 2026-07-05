---
name: web-usability
description: >-
  Apply Steve Krug's Don't Make Me Think (3rd ed.) web usability principles when
  designing or reviewing websites, web apps, navigation, copy, mobile UX, or
  running lightweight usability tests. Use for UX review, page layout, labels,
  flows, accessibility basics, or when the user mentions usability, Krug, or
  don't make me think.
---

# Web Usability — Agent Skill (Don't Make Me Think)

Rules from *Don't Make Me Think, Revisited* (Steve Krug, 2014). **Users scan and satisfice — design for that, not for careful reading.**

Pair with **web-api-design** for backend contracts and **clean-code** for implementation.

## When to apply

- Designing or reviewing web/mobile pages, flows, navigation, labels, forms
- Writing UI copy, error messages, onboarding
- Planning lightweight usability tests
- User says: usability, UX, Krug, don't make me think, make this obvious

---

## Core laws

1. **Don't make me think** — every page/question should be self-evident at a glance.
2. **Users scan, they don't read** — design billboards, not essays.
3. **Users satisfice** — first reasonable option wins; make the right choice obvious.
4. **Users muddle through** — they use what works, not what's optimal; don't rely on instructions.
5. **Omit needless words** — half the words on most pages can go.
6. **Test with real users** — early, often, cheap; fixes beat debates.

---

## Agent workflow

```
1. GLANCE TEST  — can user tell what this is and what to do in 5 seconds?
2. SCAN DESIGN  — hierarchy, headings, bold keywords, short chunks
3. CHOICES      — mindless, clearly labeled, no clever names
4. NAVIGATION   — persistent, breadcrumbs, you-are-here, search when needed
5. COPY         — cut words; kill happy talk and instructions
6. TEST PLAN    — 3 users, monthly, fix top issues
```

---

## How users actually behave (Ch 2)

- **Scan** pages for words/phrases that match their task
- **Click** the first thing that looks close enough (satisficing)
- **Back** button is the most-used control — support undo mentally
- Never assume users read instructions, compare all options, or choose optimally

Design implication: **make the right thing look clickable and obvious**.

---

## Billboard design (Ch 3)

- Clear **visual hierarchy** — what's most important looks most important
- **Use conventions** — logo top-left, nav top, search box where expected
- **Break pages into clear areas** — user decides where to look first
- **Make clickable things obvious** — buttons look like buttons; links look like links
- **Format text for scanning** — short paragraphs, bullets, bold key terms
- **Minimize noise** — every extra element competes for attention

**5-second test:** Show page 5 seconds — can they say what it's for?

---

## Mindless choices (Ch 4)

- Every click should **not require thought**
- Category names must be **mutually exclusive** and **unambiguous**
- Avoid cute/punny labels — use what users would say
- **Trunk test:** On any random page, user should instantly know: site, page, major sections, local nav, utilities, "you are here"

---

## Omit needless words (Ch 5)

Remove:
- **Happy talk** — "Welcome! We're glad you're here…"
- **Redundant instructions** — if UI is self-explanatory
- **Half the words** — then half again

Keep: words that **reduce ambiguity** or **answer a question the user has**.

---

## Navigation (Ch 6)

- **Persistent navigation** on every page (except focused flows like checkout)
- **Show location** — breadcrumbs, highlighted section, page name
- **Home** link on logo; **Search** when catalog/content is large
- **Utilities** (login, cart, help) separated from primary nav
- Site ID + tagline on every page unless user is deep in a known section

---

## Home page / landing (Ch 7)

Answer immediately:
1. What is this?
2. What can I do here?
3. What's new/important?

Don't cram everything — **prioritize** what most users need. Tagline = 6–8 words explaining what you do.

---

## Mobile (Ch 10)

- **Tradeoffs are real** — less screen = harder; don't hide essentials
- **Smart defaults**, sensible autofill, forgiving inputs
- **Learnable gestures** only when payoff is clear
- Desktop patterns don't always translate — test on device

---

## Courtesy & accessibility (Ch 11–12)

- Be a **mensch** — helpful error messages, don't punish users for mistakes
- **Accessibility helps everyone** — alt text, contrast, keyboard nav, form labels
- Fix obvious a11y issues in every review (headings, focus, link text)

---

## Usability testing (Ch 9)

**Do this:**
- **3 users** per round is enough to find major issues
- **One morning a month** — not a big lab study
- Give tasks; **stay quiet**; note where they hesitate or go wrong
- Fix **top serious problems first**; debaters lose to testers

**Don't:** wait for perfect prototype; argue instead of test.

---

## Smells to flag

| Smell | Fix |
|-------|-----|
| Clever labels / inside jargon | Plain user language |
| Wall of text | Scan-friendly chunks |
| Same-weight everything | Visual hierarchy |
| Mystery meat navigation | Obvious links, you-are-here |
| Instructions before action | Self-explanatory UI |
| Too many equal choices | Defaults, recommendations, grouping |
| No home/search on inner pages | Persistent nav |
| Untested redesign | Monthly 3-user test |

---

## Review output format

```markdown
## 5-second test
[Can user tell what this is and what to do?]

## Must fix
- [Blocks task completion or causes confusion]

## Scan & hierarchy
- [Headings, noise, clickable affordances]

## Copy
- [Words to cut or clarify]

## Navigation
- [Location, trunk test failures]

## Quick test suggestion
[1–3 tasks for next usability round]
```

---

## Source

Steve Krug, *Don't Make Me Think, Revisited* (3rd ed., 2014).
