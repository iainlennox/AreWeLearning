# README.md Format

`README.md` in the learning log directory is the dashboard: the page the human actually opens. It is
generated from the register, never edited by hand into disagreement with it.

## Template

```md
# Learning Log

What this project uses, and what is still to be learned. Maintained by the
`learning-log` skill; triage the status yourself.

Last capture: 2026-10-19 · Last review: —

## Stack

| Layer | Technologies |
| --- | --- |
| Language | C# 13 |
| Framework | ASP.NET Core 9 |
| Data | EF Core 9, SQL Server 2022 |
| Delivery | Docker, GitHub Actions |
| Process | Trunk-based development, conventional commits |

## Status

| Status | Count |
| --- | --- |
| unknown | 7 |
| learning | 2 |
| known | 3 |
| skip | 1 |
| **total** | **13** |

## Next up

1. **SKL-0003 · DI lifetimes** — used in every service; ~45 min to learn.
2. **SKL-0005 · Idempotent request handling** — affects correctness under retries.
3. **SKL-0008 · Query plans** — the list endpoint is already slow.

## Recently added

| ID | Unit | Domain | Seen |
| --- | --- | --- | --- |
| SKL-0013 | Optimistic concurrency | Data & persistence | 1 |
| SKL-0012 | Multi-stage builds | Infrastructure & delivery | 1 |
```

## Rules

- **The dashboard is derived.** Every number comes from `skills.md`. If they disagree,
  the register wins and the dashboard is wrong.
- **The stack is a snapshot, not a history.** Current state only. Drop what the project
  stopped using; history is where removals are recorded.
- **Next up holds three items.** The three a person should learn first, one line of why
  each. If everything is `known`, say so and suggest nothing.
- **Recently added is the last capture only.** No more than ten rows.
- **Empty is fine.** A log with no `learning` or `known` items still shows the stack and
  the unknown count. Do not invent activity to fill space.
