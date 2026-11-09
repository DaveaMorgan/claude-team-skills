---
name: team-setup
description: Set up a repo for the plan-driven agent team. Creates PLAN.md from a template and adds the Hard rules, Commands, Engineering standards and Team process sections to CLAUDE.md so the global dev, tech-lead, ui-reviewer, data-scientist and gtm-strategist agents and the team-lead skill work there. Use once per repo, when the user asks to set up the team, bootstrap the team process, or create a plan for agents to work.
argument-hint: "[path to a spec or PRD, optional]"
---

# Team setup

You are preparing this repo so the team can work it. The roles are global agents (`dev`, `tech-lead`, `ui-reviewer`, `data-scientist`, `gtm-strategist`) and the lead's playbook is the `team-lead` skill. They are generic. Everything project-specific comes from two files in the repo, which this skill creates or completes:

- `CLAUDE.md`: what the project is, who owns it, the Hard rules, the Commands that are the checks, the Engineering standards, and the Team process.
- `PLAN.md`: phased tasks, each with an id, **Done when** and **Needs**, plus who builds what and the open decisions.

Templates are in `${CLAUDE_SKILL_DIR}/templates/`. Input, if any: `$ARGUMENTS`.

## Steps

1. **Look first.** Run `date -u`. Read any existing `CLAUDE.md`, `PLAN.md`, README, spec or PRD, the package manifest, the CI config and `.claude/`. Never overwrite an existing file; add the missing sections and keep what is there. If the repo already has a plan in another shape (issues, a spec, a TODO), convert it instead of starting over, and say what you mapped to what.
2. **Ask only what you cannot read.** The owner's name; what the project is and for whom; the rules that must never be broken; the stack if the repo is empty; which decisions the owner wants to keep for themselves. If a spec was passed in, derive the answers from it and confirm in one message.
3. **Write `CLAUDE.md` sections** from `templates/CLAUDE-sections.md`: the project paragraph, Hard rules, Stack, Layout, Commands, Engineering standards, Working style for agents, Team process. Commands must be the real commands, checked by running them. Engineering standards are the best-practices bar for this stack, written as checkable statements; draft them and mark them for the owner to confirm.
4. **Write `PLAN.md`** from `templates/PLAN.md`. Phase 0 is always the scaffold and CI, so every later PR has checks to pass. Each task is one PR's worth of work with a testable **Done when**. Mark siblings that can run together **Parallel**. Mark anything only the owner can settle **Decision**, with a recommendation. Fill "Who builds what" by the routing rule: Sonnet for well-defined tasks, Opus for open-ended, research-heavy or security-sensitive ones.
5. **Role notes, only where needed.** If a role needs project facts that do not belong in CLAUDE.md, write `.claude/team/<role>.md` from `templates/role-notes.md`. The usual case is `ui-reviewer.md` (URL, sign-in, routes, widths). Skip roles the project does not use and leave them out of the Team process section.
6. **Check the plumbing.** `gh auth status` works; the default branch is known; CI runs the same checks as Commands on every PR; an example env file lists the config. Missing pieces become Phase 0 tasks, not things you fix now.
7. **Portability.** The global agents exist only on this machine. If the repo will be worked from cloud sessions or by other people, copy the role briefs from `~/.claude/agents/` into the repo's `.claude/agents/` and commit them. Project copies win over global ones with the same name. Ask the owner which they want; default to not copying.
8. **Hand over.** Commit on a branch named `task/0.0-team-setup` and open a PR, unless the repo is empty or the owner says to commit directly. Summarize what you wrote, list what the owner must confirm (the Hard rules, the standards, the open decisions), and say that `/team-lead` starts the work.

## What good looks like

- A dev agent who reads only `CLAUDE.md` and one task entry can build that task without asking a question.
- A tech lead can check every Hard rule and every **Done when** line by running something or searching the diff.
- Nothing in either file is a guess presented as a fact. Unknowns are **Decision** items.
