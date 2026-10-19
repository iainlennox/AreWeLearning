# Agent guide

Guidance for agents working **on this repo**. For the guidance a skill gives an agent
working **in a user's project**, see the individual `SKILL.md`.

## What this repo is

Public, curated agent skills about keeping a human in the loop while AI builds. Small,
composable, model-agnostic where possible. The bar for inclusion is in
[SCOPE.md](./SCOPE.md); the vocabulary is in [GLOSSARY.md](./GLOSSARY.md).

## Structure

```
skills/<category>/<skill-id>/
├── SKILL.md              the skill: frontmatter + procedure
├── agents/openai.yaml    Codex display metadata
├── *-FORMAT.md           format for an output the skill writes
└── *.md                  reference loaded only when a branch needs it
```

Categories are use contexts, not topics: `engineering/`, `productivity/`, `learning/`.
Add one only when a second skill needs it.

## Conventions

- **The folder name is the skill ID** and matches frontmatter `name`. Lowercase
  kebab-case, one or two words. `learning-log`, not `Learning Log`.
- **`description` is the only thing the model sees before loading the skill.** Write it
  as trigger branches in the user's own words, not a summary of features. If the model
  cannot tell when to reach for the skill, the description is broken.
- **Model-invoked by default.** Add `disable-model-invocation: true` only when the skill
  must never fire on its own.
- **`SKILL.md` holds the steps.** Reference that only some branches need goes in a
  sibling file and is linked from a table or a single line. Inline what every branch
  needs; disclose the rest.
- **Output formats get their own `*-FORMAT.md`**: where the file lives, a template in a
  fenced `md` block, then rules. One format per file so a writer loads only what it is
  writing. See `learning-log/` for the pattern.
- **Co-locate.** Scripts, references and templates sit beside `SKILL.md`; paths inside a
  skill resolve from the skill folder. No `references/` subfolder.
- **Write like the rest of the repo:** imperative, specific, no filler. Rules carry their
  reason in the same line. Prefer a concrete example to an adjective.

## Adding or renaming a skill

1. Create or move `skills/<category>/<id>/`.
2. `SKILL.md` with `name` matching the folder and a trigger-rich `description`.
3. `agents/openai.yaml` with `display_name` and `short_description`.
4. Add a row to the category `README.md` under User-invoked or Model-invoked.
5. Add a row to the root [README.md](./README.md) table.
6. Add any new repo vocabulary to [GLOSSARY.md](./GLOSSARY.md).

## Do not

- Add harness-specific config or vendor names to a skill body. Skills should survive a
  change of agent.
- Add a skill that exists only to restate another one.
- Add generated output, example project files, or secrets.
- Rewrite the vocabulary in one file without updating `GLOSSARY.md` and the skill that
  uses it.
