# AreWeLearning Glossary

The canonical language for this repo and for the logs its skills produce. Every skill
should use these words, including inside definitions of other words.

## Language

**Skill**:
A folder containing `SKILL.md` that an agent loads on demand. One skill, one job.
_Avoid_: command, workflow, prompt

**Unit**:
One thing worth learning, recorded in a register. The atom of a learning log: right-sized
when it takes 20 minutes to 2 hours to reach working understanding, and still makes sense
in another project.
_Avoid_: skill item, learning item, topic

**Altitude**:
Which of the three levels a unit sits at: **stack** (what it is made of), **craft** (how
it is made), **concept** (why it is made that way).
_Avoid_: level, tier

**Capture**:
One operation that records what a build used: scope, sweep, decompose, merge, report.
A capture writes; it does not judge.
_Avoid_: audit, scan, extraction

**Sweep**:
The evidence-gathering step of a capture. Four sources: files and manifests, commands
run, decisions made, failures debugged.
_Avoid_: scan, crawl

**Register**:
The canonical list of units in `docs/learning/skills.md`, grouped by domain. Everything
else in a log is derived from it.
_Avoid_: inventory, index, list

**Dashboard**:
The derived, human-facing view in `docs/learning/README.md`: stack snapshot, status
counts, what to learn next.
_Avoid_: summary, report

**Review**:
The triage pass: diff since the last review, ask the human for a status per unit, write
plans for the ones they mark `learning`.
_Avoid_: assessment, appraisal

**Triage**:
Assigning a unit a status during review. Only the human can do it.
_Avoid_: rate, score

**Status**:
A unit's position: `unknown` (untriaged), `learning`, `known`, or `skip`.
_Avoid_: state, progress

**Depth**:
How far into a unit someone needs to go: `awareness`, `practitioner`, `advanced`. Defaults
to `practitioner` for anything the AI implemented.
_Avoid_: level, difficulty

**Evidence**:
A real path in the project that shows a unit in use. Every register entry carries one.
_Avoid_: reference, source, link

**Plan**:
A numbered study plan for exactly one unit the human marked `learning`. Lives in
`docs/learning/plans/`.
_Avoid_: lesson, course, guide

**Roll-up**:
A personal, cross-project view of units, kept only at a path the human names. Roll up by
unit name, not ID.
_Avoid_: aggregate, master list

## Relationships

- A **capture** updates the **register** and appends to history.
- A **review** triages **units** and creates **plans**.
- The **dashboard** is derived from the **register**; it is never authoritative.
- A **plan** covers exactly one **unit**.
- A **unit** has one **status** and one **depth** at a time.

## Flagged ambiguities

- **"skill" was doing two jobs**: the repo artefact and the thing a human learns.
  Resolved: the artefact is a **skill**; the thing learned is a **unit**. This file is
  about the artefact; `docs/learning/skills.md` is about units.
- **"glossary" was doing two jobs**: this repo's vocabulary and the per-project term
  list a log produces. Resolved: this file is the **GLOSSARY**; the generated one is
  `concepts.md`.
- **"history" vs "log"**: the whole artefact set is the **log**; `history.md` is only the
  append-only record of captures and reviews.
