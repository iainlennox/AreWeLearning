# Taxonomy

Use this to classify units consistently. Domains are for grouping the register, not
for being clever. If a unit fits two domains, pick the one where the *learning* lives.

## Domains

| # | Domain | Includes | Typical units |
| --- | --- | --- | --- |
| 1 | Languages & runtimes | Syntax-adjacent semantics, type systems, memory/GC, concurrency primitives, module systems, stdlib idioms | `IQueryable` vs `IEnumerable`; async/await state machines; Go interfaces and nil receivers; Python GIL |
| 2 | Frameworks & app structure | Framework conventions, request lifecycle, composition, configuration, routing, middleware | DI lifetimes; middleware ordering; minimal API binding; Django app boundaries |
| 3 | Data & persistence | Modelling, querying, migrations, transactions, indexes, caching layers | Normalisation trade-offs; migration snapshots; N+1 queries; optimistic concurrency |
| 4 | Interfaces & integration | HTTP/REST/GraphQL design, contracts, messaging, versioning, failure handling | Idempotency keys; retry with backoff vs circuit breaking; contract-first vs code-first |
| 5 | Frontend & UX | Rendering model, state management, accessibility, bundling, styling systems | Server vs client components; controlled vs uncontrolled inputs; focus management |
| 6 | Infrastructure & delivery | Cloud primitives, containers, networking, IaC, CI/CD, environments | Multi-stage builds; managed identity; VNet/private endpoints; blue-green vs canary |
| 7 | Build & tooling | Package management, compilers, linters, formatting, repo hygiene | Lockfile semantics; build caching; monorepo vs polyrepo; semantic versioning |
| 8 | Testing & quality | Test strategy, doubles, data management, determinism, coverage | Test pyramid vs trophy; fixtures and isolation; property-based testing; flaky tests |
| 9 | Security & identity | AuthN/AuthZ, OAuth/OIDC, secrets, input handling, threat modelling | Token audience/issuer validation; least privilege; CSRF vs SameSite; secret rotation |
| 10 | Observability & ops | Logging, metrics, tracing, alerting, runbooks, cost | Structured logging; RED/USE metrics; distributed tracing context; SLOs |
| 11 | Architecture & design | Boundaries, patterns, trade-offs, decision records | Cohesion vs coupling; layered vs hexagonal; event sourcing; CQRS; ADRs |
| 12 | Process & collaboration | Version control workflow, review, estimation, documentation | Rebase vs merge; trunk-based development; conventional commits; ADRs |
| 13 | Domain & product | The actual business problem the software models | Double-entry accounting; FHIR resources; tax calculation; inventory reservation |
| 14 | AI & data | Prompting, retrieval, embeddings, evaluation, pipelines | Chunking strategy; embedding recall vs precision; eval harnesses; context windows |

Domain 13 is the one people skip and later regret. If the build encodes a business
rule, that rule is a learnable skill.

## Depth

| Depth | Meaning | Rough effort | Default for |
| --- | --- | --- | --- |
| `awareness` | Can recognise it, explain it, and know when to look it up | ~30 min | Adjacent tech encountered once |
| `practitioner` | Can use it correctly without hand-holding | hours | Anything the AI implemented for you |
| `advanced` | Can design with it and reason about trade-offs | days+ | Only if the build leaned on it heavily |

Default to `practitioner`. Do not inflate: an AI-generated implementation does not
make the human advanced in it.

## Status

| Status | Meaning |
| --- | --- |
| `unknown` | Default. Not yet triaged. |
| `learning` | Human is actively studying it; a plan may exist. |
| `known` | Human can use it unaided. **Only the human sets this.** |
| `skip` | Deliberately not learning it now. Keep it visible; do not delete. |

## Concept vs craft

- A **concept** is technology-independent: idempotency, eventual consistency,
  separation of concerns, back-pressure, blast radius.
- A **craft** unit is tied to a toolchain but still transferable: EF Core migrations,
  GitHub Actions matrices, Vitest fake timers.

Every concept gets a `concepts.md` entry. Craft units only get one when the name itself is
jargon the human would not recognise (`N+1`, `CQRS`, `blue-green`).

## Is this a skill?

Run the three tests from `SKILL.md`:

1. **Transfer** — still a skill in another codebase?
2. **Interview** — could someone ask a question with a right answer?
3. **Absence** — would a competent developer notice if it were done wrong?

Then ask: *did a human have to make a decision here?* Decisions are the richest
source of skill. `npm install` is not a skill; *choosing* a dependency strategy is.

## Naming

- Sentence case, no trailing period, no marketing words.
- Name the thing, not the action: `Optimistic concurrency`, not `Learn about locking`.
- Include the tool only when the skill is tool-specific: `EF Core migrations`,
  not `Migrations`.
- Keep under ~60 characters so tables stay readable.
