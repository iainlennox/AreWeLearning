# Scope

This repo is a curated set of skills about learning from AI-assisted work. It is not a
collection of prompts, and it is not a framework. Each skill is judged against the bar
below.

## The bar

A skill belongs here only if it clears all three:

1. **It fixes something you actually hit.** A real moment where the agent's output left
   you unable to explain, review, or maintain what it built. Hypothetical improvements
   do not count.
2. **It transfers.** The skill works in another project, another stack, and ideally
   another agent. Harness-specific configuration belongs in your config, not here.
3. **It composes.** It does one job and can be used alongside other skills without
   owning a process. Skills that try to run the whole workflow are out.

## What does not belong

- Prompt libraries and persona prompts.
- Project-specific conventions. Those belong in that project's `AGENTS.md`.
- Anything that marks a human's knowledge for them. The whole point is that they
  decide.
- A skill that restates another skill. Extend the original instead.
- Vendored tooling, generated output, or example projects.

## Changes

Issues and pull requests should describe the observed failure: what you ran, what the
skill did, what you expected. A well-argued one-line fix and a rewrite face the same bar.
