# Project plan: <Project>

<One paragraph: what is being built, for whom, and what the end state looks like.>

Read `CLAUDE.md` first. It holds the rules, definitions and standards this plan relies on.

## How to use this plan

- Each task has an ID, what to build, acceptance criteria (**Done when**), and dependencies (**Needs**).
- Take one task per PR. Tick its box in the same PR.
- Tasks marked **Parallel** can run at the same time as their siblings once their dependencies are met.
- **Decision** items are for <Owner>. Stop and ask; do not guess.
- Dev agents build, a tech-lead agent reviews, the team lead merges and reports to <Owner>. Anything outside this plan needs <Owner>'s approval first. Details in CLAUDE.md under Team process.
- Every task is built to the Engineering standards section of CLAUDE.md.

### Who builds what

Sonnet at high effort takes tasks with a clear spec and acceptance test. Opus at high effort takes open-ended, research-heavy or security-sensitive tasks. Tech-lead reviews run on Opus at high effort.

| Agent | Tasks |
|---|---|
| Opus dev | <ids, each with the reason in a few words> |
| Data scientist (Opus) | <ids, or remove this row> |
| GTM strategist (Opus) | <ids, or remove this row> |
| UI reviewer | <ids, or remove this row> |
| Sonnet dev | Every other task |
| Team lead | Every **Decision** item goes to <Owner> |

## Milestones

| Milestone | Phases | Result |
|---|---|---|
| M1: <name> | 0 to <n> | <what the owner can do when it is reached> |

---

## Phase 0: Bootstrap

- [ ] **0.1 Repo scaffold.** <Stack, tooling, folder layout from CLAUDE.md, example env file, README with setup steps.>
  **Done when:** every command under Commands in CLAUDE.md passes on a clean clone.
- [ ] **0.2 CI.** Run install and every check on each PR and on the default branch.
  **Done when:** a PR shows a green check, and a deliberately failing test turns it red.
  **Needs:** 0.1

## Phase 1: <Core logic, pure and tested first>

<One or two sentences on why this phase comes first.>

- [ ] **1.1 <Task name>.** <What to build.>
  **Done when:** <a test or reproducible check that proves it>.
  **Needs:** 0.1
- [ ] **1.2 <Task name>.** **Parallel** with 1.3. <What to build.>
  **Done when:** <check>.
  **Needs:** 1.1
- [ ] **1.4 Decision: <question>.** <Owner> decides. Recommendation: <option and why>.
  **Needs:** 1.2

---

## Open decisions for <Owner>

| ID | Question | Recommendation | Status |
|---|---|---|---|
| 1.4 | <question> | <recommendation> | Open |

When a decision is made, write `Decided <YYYY-MM-DD>: <what>` in Status and on the task itself.

## Suggested agent order

1. One agent: 0.1, then 0.2.
2. <What runs in parallel next, and what must wait for what.>
