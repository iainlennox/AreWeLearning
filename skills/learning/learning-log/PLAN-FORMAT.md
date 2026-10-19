# plans/NNNN-slug.md Format

A plan exists for exactly one unit the human marked `learning`. It is the bridge from
"the AI used this" to "I can use this". Plans are numbered sequentially in creation
order, like learning records: `0001-di-lifetimes.md`, `0002-idempotency.md`.

Scan `plans/` for the highest existing number and add one.

## Template

```md
# SKL-0003 · DI lifetimes

**Why now:** Every registered service in this app depends on the lifetime choice, and a
wrong scope is a production bug (captive dependencies, disposed contexts).
**Target depth:** practitioner · **Estimate:** 45–60 min

## Objectives
1. Explain singleton, scoped and transient, and what "scope" means in a web request.
2. Predict what breaks when a scoped service is injected into a singleton.
3. Choose a lifetime for a given service and justify it in one sentence.

## Exercise
Open `src/Api/Program.cs` and list every registration with its lifetime. Find one that
could be scoped but is singleton — or explain why none can be.

## Primary resource
Microsoft Learn — *Dependency injection in ASP.NET Core*, "Service lifetimes" section.

## Check yourself
- Why does `DbContext` default to scoped?
- What is a captive dependency, and how does the container warn you?
- When is transient the wrong default?

## Done when
You can answer the three questions above without notes, and defend the lifetimes in
`Program.cs` to a reviewer.
```

## Rules

- **One unit per plan.** A plan covering two units is two plans.
- **Why now is project-specific.** Point at the code or decision that made this worth
  learning. No generic "this is important".
- **Objectives are observable.** "Explain X", "predict Y", "choose Z and justify it".
  Never "understand X".
- **The exercise uses this codebase.** Reading about DI is not the same as auditing the
  registrations in a project you own.
- **One primary resource.** The best source found, not the first. A list of five links
  is procrastination with extra steps.
- **Self-check questions have right answers.** They are retrieval practice, not
  reflection prompts.
- **Done when is a test.** If the human cannot tell whether they are finished, the
  criterion is wrong.
- **Close the loop.** When `Done when` is met, the human sets the unit to `known` and
  the plan gets a closing line with the date. Plans are not deleted.
