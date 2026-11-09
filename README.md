# claude-team-skills

Two Claude Code skills that run a small, plan-driven agent team on any repo: a team lead that delegates every task to subagents, a tech-lead review on every PR, and a plan the owner controls.

| Skill | Use |
|---|---|
| `team-setup` | Run once per repo. Writes `PLAN.md` from a template and adds the Hard rules, Commands, Engineering standards and Team process sections to `CLAUDE.md`. |
| `team-lead` | Run each session. Works `PLAN.md` by briefing a dev agent per task, having a tech-lead agent review each PR to approval, merging when CI is green, and summarizing every PR for the owner. |

## How the team works

- The owner leads at the plan level. `PLAN.md` holds phased tasks, each with an id, a **Done when** line and a **Needs** line. Tasks marked **Parallel** run together; items marked **Decision** wait for the owner.
- A dev agent takes one task on `task/<id>-<slug>`, runs the repo's checks, ticks the box and opens a PR. A tech-lead agent reviews it and posts `Tech lead review: approved` or `changes requested`. The dev fixes until approved. The lead merges and reports.
- Nothing outside `PLAN.md` gets built without the owner's approval. Reviewers reject out-of-scope changes, even good ones, and list them for the owner.
- Routing: Sonnet at high effort for well-defined tasks, Opus at high effort for open-ended, research-heavy or security-sensitive tasks and for every review.

## Install

Copy or symlink each skill folder into `~/.claude/skills/` (personal, all repos) or a repo's `.claude/skills/` (that repo only):

```
git clone https://github.com/DaveaMorgan/claude-team-skills.git
ln -s "$PWD/claude-team-skills/skills/team-lead"  ~/.claude/skills/team-lead
ln -s "$PWD/claude-team-skills/skills/team-setup" ~/.claude/skills/team-setup
```

Then `/team-setup` in a repo, and `/team-lead` to start work.

## Agents the skills expect

The skills brief five subagent roles by name: `dev`, `tech-lead`, `ui-reviewer`, `data-scientist` and `gtm-strategist`. Their definitions are not in this repo. Put them in `~/.claude/agents/` (all repos) or `.claude/agents/` (one repo); a project copy wins over a personal one with the same name. Project-specific notes for a role go in `.claude/team/<role>.md`, which each brief reads if present.

## Layout

```
skills/team-lead/SKILL.md
skills/team-setup/SKILL.md
skills/team-setup/templates/PLAN.md
skills/team-setup/templates/CLAUDE-sections.md
skills/team-setup/templates/role-notes.md
```
