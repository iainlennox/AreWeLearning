# Extraction

The sweep has four sources. Most captures fail because the agent only looks at files
and misses the two richest sources: **what it ran** and **what it decided**.

## 1. Files

Read names before contents. Names carry most of the signal; open files only when a
name is ambiguous.

| Signal | Reveals |
| --- | --- |
| Extensions (`.cs`, `.py`, `.ts`, `.go`, `.rs`, `.sql`, `.bicep`) | Languages, and the mix of concerns |
| `*.csproj`, `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `pom.xml`, `Gemfile`, `composer.json` | Frameworks, runtime versions, dependency strategy |
| Lockfiles (`package-lock.json`, `poetry.lock`, `go.sum`, `packages.lock.json`) | Whether dependencies are pinned — a craft unit in itself |
| `Dockerfile`, `docker-compose.yml`, `.dockerignore` | Containers, base-image choice, multi-stage builds |
| `*.bicep`, `*.tf`, `k8s/**`, `helm/**`, `serverless.yml`, `azure.yaml` | IaC, deployment model, environment topology |
| `.github/workflows/**`, `azure-pipelines.yml`, `.gitlab-ci.yml` | CI/CD, gates, secrets handling, matrix builds |
| `**/migrations/**`, `*.sql`, `schema.prisma`, `alembic.ini` | Schema versioning, data modelling |
| `appsettings*.json`, `.env.example`, `config/**` | Configuration binding, environment separation |
| `tsconfig.json`, `vite.config.*`, `webpack.config.*`, `.eslintrc*`, `.editorconfig` | Build tooling, lint/format policy |
| `**/*Tests/**`, `*.test.*`, `*.spec.*`, `conftest.py`, `Testcontainers` | Test strategy and doubles |
| `README.md`, `docs/adr/**`, `CONTRIBUTING.md` | Stated process and decisions — verify against reality |
| `Program.cs`, `main.*`, `app/**/layout.*`, `middleware/**` | Composition and request lifecycle |

Prefer a quick histogram of extensions over opening every directory:

```sh
git ls-files | sed 's/.*\.//' | sort | uniq -c | sort -rn | head -20
```

## 2. Commands run

Whatever the agent executed is evidence of craft, and often of skills the human
never saw:

| Command pattern | Candidate units |
| --- | --- |
| `dotnet ef migrations …` | Migrations and model snapshots |
| `dotnet user-secrets`, `az keyvault secret` | Local secret management |
| `docker build --target …`, `docker compose up` | Multi-stage builds, service topology |
| `terraform plan/apply`, `az deployment …` | Plan-before-apply, idempotent infrastructure |
| `npm run build`, `dotnet publish -c Release` | Build configuration, trimming/AOT |
| `git rebase`, `git cherry-pick`, `git bisect` | History workflow, debugging by bisection |
| `curl` against an endpoint | Contract shape, auth flow |
| test runner with filters | Test selection, isolation |
| `EXPLAIN`, `pg_stat_activity`, profiler | Query plans, connection pooling |

Do not log the command itself. Log the practice it demonstrates.

## 3. Decisions and trade-offs

The highest-value source. Look for moments where the agent chose one option over
another, and record the *axis* of the choice:

- "chose SQLite over Postgres" → embedded vs server databases, when each is right
- "used a background worker instead of inline work" → decoupling, at-least-once delivery
- "put the retry in the client" → retry amplification, idempotency requirements
- "added an index" → read/write trade-offs, query planning
- "kept it in one project" → modular monolith vs distributed boundaries
- "stubbed the external call in tests" → test seams and determinism

If a decision was made *for* the human without discussion, that is exactly what the
log exists to surface. Say so plainly in the entry's *why it mattered*.

## 4. Failures and fixes

Debugging episodes produce the most memorable concepts. Map the symptom to the idea:

| Symptom | Concept |
| --- | --- |
| Browser blocks a request across origins | CORS preflight, credentialed requests |
| 401 in production, 200 locally | Token audience/issuer, clock skew, environment config |
| Works locally, fails in a container | Config binding, DNS/service discovery, filesystem assumptions |
| Slow list endpoint | N+1 queries, missing index, over-fetching |
| Flaky test | Test isolation, shared state, time and ordering |
| "It worked, then stopped" after a merge | Dependency resolution, lockfiles, cache invalidation |
| Deploy succeeded, app unhealthy | Health probes, startup ordering, readiness vs liveness |
| Costs jumped | Egress, over-provisioned SKUs, chatty APIs |

## Guardrails for the sweep

- **A dependency is not a skill.** Only log it if the code uses it in a way worth
  understanding. `Serilog` in a project file → stack line. Structured logging with
  enrichers and sinks → a craft unit.
- **Boilerplate is not a skill.** Generated CRUD scaffolding is not a unit; the
  pattern it follows might be.
- **Don't infer intent from names alone.** `helpers.ts` with 400 lines of date maths
  is a decision about organisation, not a skill named "helpers".
- **Mark uncertainty.** If you cannot point at evidence, write `(inferred)` and leave
  it out of the register until the human confirms.
- **Cap the session.** All meaningful stack items, then at most ~15 craft/concept
  units. Name what you dropped.

## Worked example

A session adds a `/orders` endpoint with a database write, a retry, a Dockerfile and
a CI workflow.

| Evidence | Unit | Domain | Depth |
| --- | --- | --- | --- |
| `Orders.csproj`, `Program.cs` | Minimal API endpoint binding and validation | Frameworks | practitioner |
| `Migrations/20261019_Init.cs` | EF Core migrations and model snapshots | Data | practitioner |
| Retry policy on the HTTP client | Retry with backoff vs circuit breaking | Integration | practitioner |
| `Idempotency-Key` header handling | Idempotent request handling | Architecture | practitioner |
| `Dockerfile` with `build` and `runtime` stages | Multi-stage builds and layer caching | Infrastructure | practitioner |
| Workflow with `dotnet test` on PR | CI gates and branch protection | Process | awareness |

Six units. The `Serilog` package and the `.editorconfig` stay in the stack snapshot,
with no skill entry.
