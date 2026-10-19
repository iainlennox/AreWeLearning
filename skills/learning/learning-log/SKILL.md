---
name: learning-log
description: "Capture what a build actually used and turn it into a study list. Writes docs/learning/ with the stack, the craft and the concepts the work exercised, so the human can review what the AI did on their behalf and learn the gaps. Use after a build or substantial change, or when the user says \"log what this used\", \"update my learning log\", \"what did we just use\", \"what should I learn next\", or \"review my gaps\"."
---

# Learning Log

A build is a curriculum nobody wrote down. This skill writes it down: an honest,
deduplicated inventory of what the work exercised, so the human can see what they now
own but cannot yet explain. It is the reference that keeps that inventory useful —
what a unit is, where it lives, what to record, and what to leave out.

A **unit** is one thing worth learning. The log is a list of units, not a list of
technologies. `C#` is a technology; dependency injection lifetimes are a unit.

The log is a **mirror, not a tutorial**. It answers one question well: *what did this
build use that I should be able to explain or do myself?*

## The workspace

The log lives in `docs/learning/` at the project root:

- `README.md`: the dashboard. Stack snapshot, status counts, what to look at next. Use [DASHBOARD-FORMAT.md](./DASHBOARD-FORMAT.md).
- `skills.md`: the register. One entry per unit, grouped by domain, IDs permanent. Use [REGISTER-FORMAT.md](./REGISTER-FORMAT.md).
- `concepts.md`: the terms met while building, defined in three lines each. Use [CONCEPTS-FORMAT.md](./CONCEPTS-FORMAT.md).
- `history.md`: an append-only record of captures and reviews. Use [HISTORY-FORMAT.md](./HISTORY-FORMAT.md).
- `plans/0001-<slug>.md`: a study plan, one per unit the human marks `learning`. Use [PLAN-FORMAT.md](./PLAN-FORMAT.md).
- `archive.md`: retired units, kept but no longer counted.

If a log already exists elsewhere, use it. Never create a second one. Migrate an
existing log in place: keep every entry and every ID, then normalise the format.

## What counts as a unit

Capture at three altitudes. A good session produces some of each.

| Altitude | Answers | Example |
| --- | --- | --- |
| **Stack** | What is it made of? | C# 13, ASP.NET Core, SQL Server, Docker |
| **Craft** | How is it made? | EF Core migrations, DI lifetimes, retry policies |
| **Concept** | Why is it made that way? | Idempotency, optimistic concurrency, cohesion |

A unit is right-sized when a motivated person could reach working understanding in
20 minutes to 2 hours, and it would still make sense on a different project.

| Too broad | Too narrow | Unit |
| --- | --- | --- |
| C# | the `var` keyword | LINQ deferred execution: `IQueryable` vs `IEnumerable` |
| SQL | `SELECT` syntax | Reading a query plan and choosing an index for a filtered join |
| ASP.NET Core | `app.MapGet` | Minimal API binding, validation and endpoint filters |
| Testing | `[Fact]` | Fakes vs mocks: choosing test seams in a layered app |
| Docker | `docker build` | Multi-stage builds and layer-cache invalidation |
| "the cloud" | `az group create` | Managed identity vs connection strings for service-to-service auth |

Three tests for a candidate unit:

- **Transfer** — would it still be a skill in a different codebase?
- **Interview** — could someone ask a question about it that has a right answer?
- **Absence** — would a competent developer notice if it were done wrong?

If it fails all three, drop it or fold it into a parent unit. If a dependency is only
present, not used in a way worth understanding, it is a stack line and nothing more.

In a **baseline**, favour the units the project leans on — the patterns repeated across
many files — over one-off details. A baseline that lists every library has failed; a
baseline that names the ten ideas holding the system together has worked.

## Capture

### 1. Scope

Two kinds of capture:

- **Baseline** — the first capture in a project that already exists. Scope is the whole
  repository, not a diff. Every module gets a stack line; units come from the
  architecture and the choices already baked in. Expect more stack and fewer units than
  an incremental capture.
- **Incremental** — every capture after that. Scope is what changed: `git status`,
  `git diff --stat`, and the conversation. Uncommitted work is fine; say so in the
  history entry instead of inventing a commit range.

If you cannot tell which applies, ask. Never describe a baseline as a diff, or a diff as
a baseline.

### 2. Sweep for evidence

Four sources, all mandatory: **files and manifests**, **commands run**, **decisions
made**, **failures debugged**. [sweep.md](./sweep.md) maps signals to units.
The richest source is the choice the agent made without asking — that is what the log
exists to surface.

### 3. Decompose

Convert evidence into candidate units. Stack completeness, craft/concept selectivity:
every meaningful technology, but only the ~10–15 most consequential units. Name what
you dropped, in one line.

### 4. Merge into the register

- Match by meaning, not wording. `DI scopes` and `service lifetimes` are one unit.
- Existing unit: increment `Seen`, update `Last`, touch the text only when the new use
  adds a nuance. Never reset `Status`.
- New unit: take the next ID from `next_id`, default `Status: unknown`.
- Domain and depth come from [taxonomy.md](./taxonomy.md). Use the vocabulary there.

### 5. Define the concepts

Every concept unit gets an entry in `concepts.md`, and so does any craft unit whose name
is jargon. Three lines maximum, linked to its ID. Definitions describe the idea; they
do not imply the human has learned it.

### 6. Refresh the dashboard and append history

Recount by status, refresh the stack snapshot, add a history block. Do not touch
`last_review` — capture is not review.

### 7. Report

Under ten lines:

```
Added 7 units (all unknown), deepened 2, skipped 3.
  New: EF Core migrations, DI lifetimes, idempotent endpoints, …
Most worth your time: DI lifetimes (used everywhere, cheap to learn).
Triage: say which of the 7 you already know.
```

Capture is done when every new unit has real evidence, every concept has a definition,
and the counts in the dashboard match the register.

## Review

Review is the point of the exercise. It runs on request, never as a side effect.

1. Diff the register against `last_review` in its frontmatter.
2. Classify: **new**, **deepened**, **untouched**, **stale** (not seen in 12 months).
3. Show at most ten units at a time, ranked by *frequency of use × foundational-ness ×
   cost to learn*.
4. Ask for a status per unit: `known`, `learning`, `skip`. Never infer one.
5. For each `learning`, write a numbered plan with an exercise in **this** codebase,
   one primary resource, and self-check questions.
6. Set `last_review` to today and append a review block to history.

Never bulk-mark units as `known`. A register that is 100% known is a register nobody
read.

## Rules

- **Evidence over guesswork.** Every entry points at something real in the repo. Mark
  guesses `(inferred)` and keep them out of the register until confirmed.
- **The human owns status.** Only they set `known`. Anything new stays `unknown` until
  they say otherwise.
- **Merge, don't rewrite.** History is append-only, IDs are permanent, entries are
  updated in place.
- **Terse.** One line of why, one line of what to understand. If an entry runs past five
  lines, it belongs in a plan.
- **No secrets.** Never copy tokens, keys, connection strings or customer data into the
  log, not even as examples.
- **No lecture.** No flattery, no commentary on the human's skill level, no tutorial
  dumping. State what was used and let them decide what it means.
- **No scope creep.** Write only inside `docs/learning/`. Never touch source, README or
  config as part of a capture.
- **Respect a no.** If the human declines a capture, do not ask again that session.

## Across projects

A project log is the unit of evidence; keep it there. If the human wants a personal,
cumulative view, write it only to a path they name, and only when asked. Roll up by unit
name, not ID — the same unit can be `SKL-0003` here and `SKL-0011` there — and track how
many projects exercised it. Seen in five projects beats seen five times in one.

## Reference

| File | Read when |
| --- | --- |
| [taxonomy.md](./taxonomy.md) | Choosing a domain or depth; checking whether something is a unit |
| [sweep.md](./sweep.md) | Running the sweep; mapping signals to units |
| [DASHBOARD-FORMAT.md](./DASHBOARD-FORMAT.md) | Writing or updating `README.md` |
| [REGISTER-FORMAT.md](./REGISTER-FORMAT.md) | Writing or updating `skills.md` |
| [CONCEPTS-FORMAT.md](./CONCEPTS-FORMAT.md) | Writing or updating `concepts.md` |
| [HISTORY-FORMAT.md](./HISTORY-FORMAT.md) | Appending a capture or review block |
| [PLAN-FORMAT.md](./PLAN-FORMAT.md) | Writing a study plan |
