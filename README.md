# AreWeLearning

Skills for modern AI development.

A small, curated collection of agent skills that keep a human in the loop while AI does
the building — not by slowing the work down, but by making sure what was built can still
be understood afterwards.

Works with Claude Code, Codex, GitHub Copilot, OpenCode, or anything that reads a
`SKILL.md`.

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

## How it works

Two directories, one clone:

```text
~/code/AreWeLearning                 ← clone once. The library.
  skills/learning/learning-log/
    SKILL.md                         ← registers as "learning-log"

~/.config/opencode/opencode.json     ← one line: where the library lives
  "skills": ["/home/you/code/AreWeLearning"]
```

The **folder name is the skill ID**. The name of the clone directory is irrelevant:
discovery finds any `SKILL.md` at any depth, so
`AreWeLearning/skills/learning/learning-log/SKILL.md` registers as `learning-log`, not
`AreWeLearning`.

Nothing runs from this repo. You open your agent in the project you want to audit, and
the skill reads and writes there. Only the registration differs by harness — a `skills`
array for OpenCode, `copilot skill add` for Copilot CLI, a scanned directory for Claude
Code — and all of them point at the same library.

## Install

### Point your config at a clone (recommended)

```sh
git clone https://github.com/iainlennox/AreWeLearning.git ~/code/AreWeLearning
```

```jsonc
// ~/.config/opencode/opencode.json (global) or opencode.jsonc (project)
{
  "$schema": "https://opencode.ai/config.json",
  "skills": ["/home/you/code/AreWeLearning"]
}
```

Use the global config so every project can reach it; use a project `opencode.jsonc` when
only one repo should see it. Prefer an absolute path — relative paths resolve from the
session's working directory, which is your target project, not this repo.

Start a new session if the skill is not picked up immediately. Update with `git pull` in
the clone; every project sees the change at once.

### Clone into a skills directory

If your harness already scans one, no config is needed:

```sh
# OpenCode global, one project, or Claude Code
git clone https://github.com/iainlennox/AreWeLearning.git \
  ~/.config/opencode/skills/AreWeLearning
git clone https://github.com/iainlennox/AreWeLearning.git \
  .opencode/skills/AreWeLearning
git clone https://github.com/iainlennox/AreWeLearning.git \
  ~/.claude/skills/AreWeLearning
```

### GitHub Copilot CLI

Copilot CLI reads the same `SKILL.md` standard. Register the category folder once:

```sh
copilot skill add /path/to/AreWeLearning/skills/learning
copilot skill list
```

That writes a pointer to `~/.copilot/settings.json`, so `git pull` updates Copilot and
OpenCode together. The directory must contain skill folders directly, which is why it is
`skills/learning` and not the repo root.

Or let GitHub CLI install and track it:

```sh
gh skill install iainlennox/AreWeLearning learning-log
```

That copies the files; `gh skill` handles placement and updates.

### Vendor a single skill

To pin one skill to one project and share it with the team, copy the folder:

```sh
cp -r skills/learning/learning-log /path/to/project/.opencode/skills/  # OpenCode
cp -r skills/learning/learning-log /path/to/project/.github/skills/    # Copilot, incl. cloud agent
```

Do not clone this repo inside another repo — you would end up with a nested checkout.
Copy the folder instead.

## Use

Open your agent **in the project you want to audit**:

```sh
cd ~/Work/distributedinferencelive
opencode
```

Then ask, either with a trigger phrase:

```
baseline this repo        # first run on an existing codebase
log what this used        # after a feature or refactor
review my gaps            # triage what is still unknown
```

or name the skill explicitly when you want to be certain:

```
@learning-log baseline this repo          # OpenCode
Use the /learning-log skill to baseline this repo.   # Copilot CLI
```

`@learning-log` loads the skill; the words after it are the job. Naming the skill on its
own tends to get you a "what would you like me to do?". Copilot CLI addresses skills with
a leading slash instead of an at-sign.

Capture writes to `docs/learning/` in the project you are working in — or
`Docs/learning/` if the project already uses that casing. Never to this repo, never to
source files.

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
