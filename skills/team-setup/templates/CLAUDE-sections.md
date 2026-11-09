# <Project>

<Two or three sentences: what it is, who it is for, who owns it. Name the owner; the agents refer to them as the owner.>

The work plan lives in `PLAN.md`. Read it before starting, take one task at a time, and tick its checkbox in the same PR that finishes it.

## Hard rules

<The short list of things that must never happen. Each one checkable by a reviewer. Include the writing rules for copy, commits and PRs if the owner has any.>

- <Rule.>
- Never commit secrets. Config comes from environment variables listed in `.env.example`.

## Stack

- <Language, runtime, package manager, framework, database, hosting, test and lint tools, CI.>

## Layout

```
<folder>/    <what lives there>
```

## Commands

These are the checks. Dev agents run all of them before opening a PR and the tech lead runs them again.

```
<command>    # typecheck
<command>    # lint
<command>    # tests
<command>    # build
```

## Domain definitions

<Terms and formulas an agent must not improvise. Remove the section if there are none.>

## Engineering standards

This is the best-practices bar. Dev agents build to it and the tech lead reviews against it. It shapes how tasks are built, not what gets built; PLAN.md still decides scope.

- <Checkable statement per area: structure, data access, validation, error and empty states, security, tests, CI gates, dependencies.>

## Working style for agents

- Small PRs, one PLAN.md task each. Branch name: `task/<id>-<slug>`.
- Core logic first, tested in isolation, before UI.
- If a sandbox cannot reach a service the task needs, use fixtures and say so in the PR. Do not fake data.
- If a task needs a decision only <Owner> can make (marked **Decision** in PLAN.md), stop and ask instead of guessing.

## Team process

This repo runs the plan-driven agent team. The team lead follows the `team-lead` skill. The roles are the agents of the same names; notes for a role that are specific to this project live in `.claude/team/<role>.md`.

- **Dev agents** take one PLAN.md task each, build it on `task/<id>-<slug>`, run every check under Commands, tick the task's checkbox, and open a PR.
- **The tech-lead agent** reviews each PR against PLAN.md and this file and either approves or requests changes. The dev agent addresses feedback until the tech lead approves.
- **The ui-reviewer agent** reviews the running UI as a senior product designer and returns ranked findings; it does not build. <Remove if unused.>
- **The data-scientist agent** builds model tasks with out-of-sample discipline and opens a PR like a dev; the tech lead reviews it against the model checklist. <Remove if unused.>
- **The gtm-strategist agent** researches positioning and competitors and returns a sourced doc with ranked recommendations; it does not build. <Remove if unused.>
- **The team lead** (the top-level session) delegates all hands-on work, merges approved PRs once CI is green and gives <Owner> a summary of every PR.
- **Stay inside PLAN.md.** Any change that adds, removes or reshapes a task, a table, a dependency, a formula or a user-facing feature needs <Owner>'s approval before it is built. Reviewers reject out-of-scope changes even when they are improvements, and note them for <Owner> instead.
- **Model routing.** Well-defined tasks (a clear spec, an acceptance test or a reference to match) go to a Sonnet dev agent at high effort. Open-ended tasks that need research or design, and anything security-sensitive, go to an Opus dev agent at high effort. Tech-lead reviews run on Opus at high effort. The table in PLAN.md under "Who builds what" assigns each task.
