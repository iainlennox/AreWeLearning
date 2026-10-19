# history.md Format

`docs/learning/history.md` is the append-only record of what the log has seen. It is
not a journal of the project — only captures and reviews of the log.

## Template

```md
# Capture history

Newest first. One block per capture or review. Never edit an older block.

## 2026-10-19 · Review
- Triaged 10 units: 3 known, 2 learning, 1 skip, 4 unknown.
- Plans created: `plans/0001-di-lifetimes.md`, `plans/0002-idempotency.md`.
- Last review set to 2026-10-19.

## 2026-10-19 · Initial build: order API
- Scope: `ba2dec3..HEAD`, 41 files, uncommitted
- Stack: C# 13, ASP.NET Core 9, EF Core 9, SQL Server 2022, Docker, GitHub Actions
- Added: SKL-0001–SKL-0013 (13 units, all `unknown`)
- Deepened: none
- Skipped: Swashbuckle, `.editorconfig` (dependency/config only, nothing to learn)
```

## Rules

- **Newest first.** New blocks go directly under the intro line.
- **Append only.** Never rewrite or delete a block. A correction is a new block that
  says what changed.
- **One heading per event.** `## <date> · Capture: <what was built>` or
  `## <date> · Review`.
- **State the scope.** `baseline, 564 files` or a commit range or "uncommitted", plus
  the file count. Never invent a range.
- **Report movement, not activity.** Units added, deepened, skipped and why. If nothing
  was added, say so — a capture with no new units is a useful result.
- **Name what was dropped.** One line, with the reason. It prevents the same argument
  next session.
