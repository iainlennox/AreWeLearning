# AreWeLearning

Skills for modern AI development.

A small, curated collection of agent skills that keep a human in the loop while AI does
the building — not by slowing the work down, but by making sure what was built can still
be understood afterwards.

Works with Claude Code, Codex, OpenCode, or anything that reads a `SKILL.md`.

## Skills

| Category | Skill | What it does |
| --- | --- | --- |
| Learning | [`learning-log`](skills/learning/learning-log/SKILL.md) | After a build, writes an honest inventory of the stack, craft and concepts it used, so you can review what the AI did on your behalf and study the gaps. |

## The problem

AI can produce a working app faster than you can learn what is in it. A month later the
stack is a stranger: you own code you cannot explain, and the interview for your own
project goes badly.

`learning-log` turns that into a reviewable artefact. Every build leaves a
`docs/learning/` folder in your project:

- **Stack** — what the thing is made of (C#, ASP.NET Core, SQL Server, Docker)
- **Craft** — how it is made (EF Core migrations, DI lifetimes, retry policies)
- **Concept** — why it is made that way (idempotency, optimistic concurrency, cohesion)

You triage each unit as known, learning, or skip. The ones you mark *learning* get a
short plan with an exercise in your own codebase. Nothing is ever marked "known" for
you.

### What it looks like

```markdown
### SKL-0002 · Idempotent request handling
**Domain:** Architecture & design · **Depth:** practitioner · **Status:** unknown
**Seen:** 1 · **Last:** 2026-10-19 · **Evidence:** `src/Api/Endpoints/Orders.cs:44`

**Why it mattered:** The endpoint accepts repeated POSTs with the same key, which is
only safe because the handler is idempotent.
**What to understand:** Idempotency keys, at-least-once delivery, and where to store
the key so a retry returns the original result.
```

## Install

Clone into a directory your agent already discovers:

```sh
# Global (OpenCode; use ~/.claude/skills for Claude Code)
git clone https://github.com/iainlennox/AreWeLearning.git \
  ~/.config/opencode/skills/AreWeLearning

# Project-local
git clone https://github.com/iainlennox/AreWeLearning.git \
  .opencode/skills/AreWeLearning
```

Discovery finds any `SKILL.md` at any depth, so the whole repo can be cloned and each
skill still registers by its folder name.

Prefer to keep the repo elsewhere? Point your config at it:

```jsonc
// opencode.jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "skills": ["./AreWeLearning"]
}
```

Or take a single skill:

```sh
cp -r skills/learning/learning-log ~/.claude/skills/
```

Update with `git pull` in the clone.

## Use

Ask for it, or let the agent offer at the end of a build:

```
log what this used
update my learning log
what should I learn next
review my gaps
```

Capture writes to `docs/learning/` in the project you are working on — never to this
repo, and never to your source files.

## Repository layout

```text
.
├── AGENTS.md                  conventions for agents working on this repo
├── GLOSSARY.md                this repo's vocabulary: unit, capture, register, triage
├── SCOPE.md                   what belongs here, and the bar it has to clear
└── skills/
    └── learning/
        ├── README.md          category index
        └── learning-log/
            ├── SKILL.md       the skill: frontmatter + procedure
            ├── agents/
            │   └── openai.yaml
            ├── taxonomy.md    domains, depth, status, granularity
            ├── sweep.md       how to detect what a build actually used
            ├── DASHBOARD-FORMAT.md
            ├── REGISTER-FORMAT.md
            ├── CONCEPTS-FORMAT.md
            ├── HISTORY-FORMAT.md
            └── PLAN-FORMAT.md
```

`SKILL.md` stays short enough to load on every use. Everything else is disclosed: the
agent reads a format file only when writing that file, and reads the sweep only during a
capture.

## Conventions

- The folder name is the skill ID, and matches frontmatter `name`.
- A description is written as trigger branches, because it is the only thing the model
  sees before loading the skill.
- Every output format gets its own `*-FORMAT.md`: where it lives, a template, then
  rules.
- Reference that only some branches need is co-located and linked, never inlined.

Full detail in [AGENTS.md](./AGENTS.md). The bar for a new skill is in
[SCOPE.md](./SCOPE.md). The vocabulary is in [GLOSSARY.md](./GLOSSARY.md).

## License

[MIT](./LICENSE).
