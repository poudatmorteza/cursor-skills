# Cursor Agent Skills

Book-distilled [Cursor Agent Skills](https://cursor.com/docs/context/skills) for software development — coding standards, architecture, and full-stack web work.

**Not included:** trading/forex skills (kept private locally).

## Skills

| Skill | Source |
|-------|--------|
| [clean-code](./clean-code/) | Robert C. Martin — *Clean Code* (2nd ed.) |
| [design-patterns](./design-patterns/) | Gamma et al. — *Design Patterns* (GoF) |
| [domain-driven-design](./domain-driven-design/) | Vlad Khononov — *Learning Domain-Driven Design* |
| [software-architecture](./software-architecture/) | Richards & Ford — *Fundamentals of Software Architecture* |
| [ddia](./ddia/) | Martin Kleppmann — *Designing Data-Intensive Applications* |
| [web-api-design](./web-api-design/) | Arnaud Lauret — *The Design of Web APIs* |
| [web-usability](./web-usability/) | Steve Krug — *Don't Make Me Think* (3rd ed.) |
| [web-application](./web-application/) | Combined full-stack skill (all four web books above) |
| [karpathy-guidelines](./karpathy-guidelines/) | Andrej Karpathy — LLM coding behavioral guidelines ([source](https://github.com/multica-ai/andrej-karpathy-skills)) |

## Install

Copy the skill folders you want into your Cursor skills directory:

```bash
git clone https://github.com/poudatmorteza/cursor-skills.git
cp -R cursor-skills/*/ ~/.cursor/skills/
```

Or install one skill:

```bash
cp -R cursor-skills/clean-code ~/.cursor/skills/
```

Cursor loads skills from `~/.cursor/skills/<name>/SKILL.md` (and project-level `.cursor/skills/`).

## Usage

Skills activate automatically when the task matches their description (e.g. "review this API", "apply DDD", "clean code refactor"). You can also `@`-mention a skill in Cursor chat.

## License

Skill content is original synthesis for agent assistance. Underlying books remain the property of their respective authors and publishers — buy and read the books for the full treatment.
