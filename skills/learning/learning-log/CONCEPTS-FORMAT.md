# concepts.md Format

`docs/learning/concepts.md` defines the jargon the project uses. It is a study aid,
not a certificate: an entry means the term was met, never that the human has learned it.

## Template

```md
# Concepts

Terms met while building this project. Three lines each; follow the ID for depth.

## I

### Idempotency
Repeating an operation has the same effect as doing it once. Matters whenever a request
can be retried: networks, queues, users double-clicking.
→ SKL-0002

## O

### Optimistic concurrency
Detect a conflicting write at commit time with a version or timestamp check, instead of
locking up front. Trades occasional retries for throughput.
→ SKL-0013
```

## Rules

- **Alphabetical, grouped under letter headings.** `## I`, `## O`, and so on.
- **One definition, one consequence.** What the term is, then why it matters here. If a
  third line is needed, it is a link or a plan, not more prose.
- **Three lines maximum.** Long explanations belong in `plans/`.
- **Link the unit.** Every entry ends with `→ SKL-nnnn`. A term with no unit either
  becomes one or does not belong in the file.
- **Use the file's own vocabulary.** Once a term is defined here, every other entry and
  every plan uses it, including inside other definitions.
- **No marketing gloss.** Define the concept, not the vendor's version of it.
