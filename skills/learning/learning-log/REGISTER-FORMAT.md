# skills.md Format

`docs/learning/skills.md` is the register: the source of truth. Everything else in the
workspace is generated from it or points into it.

## Frontmatter

```yaml
---
next_id: 14
last_review: 2026-10-19
updated: 2026-10-19
---
```

- `next_id` is the next free number and is bumped on every addition. IDs are never
  reused, even after a unit is archived.
- `last_review` is the date of the last review, not the last capture. Leave it empty
  until a review happens.
- `updated` is the date of the last write of any kind.

## Template

```md
# Skill register

One entry per learnable unit. IDs are permanent. `Status` is set by the human only.

## Data & persistence

### SKL-0001 · EF Core migrations
**Domain:** Data & persistence · **Depth:** practitioner · **Status:** unknown
**Seen:** 3 · **Last:** 2026-10-19 · **Evidence:** `src/Data/Migrations/20261019_Init.cs`

**Why it mattered:** Schema changes were applied through generated migrations, so the
model snapshot — not SQL — is the source of truth.
**What to understand:** How a migration is diffed from the model, why snapshots exist,
and how to apply migrations safely in production (idempotent scripts, expand/contract).

## Architecture & design

### SKL-0002 · Idempotent request handling
**Domain:** Architecture & design · **Depth:** practitioner · **Status:** unknown
**Seen:** 1 · **Last:** 2026-10-19 · **Evidence:** `src/Api/Endpoints/Orders.cs:44`

**Why it mattered:** The endpoint accepts repeated POSTs with the same key, which is
only safe because the handler is idempotent.
**What to understand:** Idempotency keys, at-least-once delivery, and where to store
the key so a retry returns the original result.
```

## Rules

- **Domains follow the taxonomy order** and appear only when populated. The full list is
  in [taxonomy.md](./taxonomy.md).
- **`Seen` increments** every capture that exercises the unit. `Last` is that capture's
  date. Neither is ever decremented.
- **`Status` is monotonic by consent only.** `unknown → learning → known` happens when
  the human says so. `skip` can be set from any state and reversed.
- **`Evidence` is a real path**, with `:line` only when certain. End it with `(inferred)`
  when the link is a guess, and do not add the unit to the register until confirmed.
- **Two lines of prose, then stop.** *Why it mattered* is specific to this project.
  *What to understand* is transferable. Anything longer is a plan.
- **Never delete.** Move a retired unit to `archive.md` and leave a note in history.
