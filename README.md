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

The skills and the agents are both needed. Clone the repo, then link each piece into your personal Claude Code folders so every repo on the machine gets them:

```
git clone https://github.com/DaveaMorgan/claude-team-skills.git
cd claude-team-skills

# the two skills
ln -s "$PWD/skills/team-lead"  ~/.claude/skills/team-lead
ln -s "$PWD/skills/team-setup" ~/.claude/skills/team-setup

# the five agents the skills brief by name
mkdir -p ~/.claude/agents
for f in agents/*.md; do ln -s "$PWD/$f" ~/.claude/agents/$(basename "$f"); done
```

Symlinks keep the installed copies in step with the repo when you pull. Copying the files instead also works.

Check it: start Claude Code in any folder and ask it to list its agent types and skills. It should name `dev`, `tech-lead`, `ui-reviewer`, `data-scientist`, `gtm-strategist`, `team-lead` and `team-setup`.

Then run `/team-setup` in a repo, and `/team-lead` to start work.

### Installing for one repo only, or for cloud sessions

Personal folders exist only on your machine. To make the team available to collaborators or to Claude Code sessions in the cloud, commit copies into the repo itself:

```
cp -R claude-team-skills/skills/team-lead  <repo>/.claude/skills/team-lead
cp -R claude-team-skills/skills/team-setup <repo>/.claude/skills/team-setup
mkdir -p <repo>/.claude/agents && cp claude-team-skills/agents/*.md <repo>/.claude/agents/
```

Name clashes resolve differently for the two kinds: a project agent in `.claude/agents/` wins over a personal one with the same name, but a personal skill in `~/.claude/skills/` wins over a project skill with the same name.

## The agents

| Agent | Model | Does |
|---|---|---|
| `dev` | Sonnet, high effort (the lead switches to Opus for open-ended or security-sensitive tasks) | Builds one `PLAN.md` task on `task/<id>-<slug>`, runs the repo's checks, ticks the box, opens a PR |
| `tech-lead` | Opus, high effort | Reviews a PR against `PLAN.md` and `CLAUDE.md` in a fixed order; posts the verdict as a review whose first line is `Tech lead review: approved` or `changes requested` |
| `ui-reviewer` | Opus, high effort | Reviews the running UI phone first and returns ranked findings with screenshots; does not build |
| `data-scientist` | Opus, high effort | Builds model and analysis tasks with point-in-time and out-of-sample discipline; opens a PR like a dev |
| `gtm-strategist` | Opus, high effort | Researches positioning and competitors; returns a sourced doc with ranked recommendations; does not build |

Each brief is generic. It reads the repo's `CLAUDE.md` for the Hard rules, the Commands that are the checks and the Engineering standards, and reads `.claude/team/<role>.md` for project-specific notes if that file exists. Every role that writes to the repo runs in its own git worktree (`isolation: worktree` in the frontmatter).

## Layout

```
agents/dev.md
agents/tech-lead.md
agents/ui-reviewer.md
agents/data-scientist.md
agents/gtm-strategist.md
skills/team-lead/SKILL.md
skills/team-setup/SKILL.md
skills/team-setup/templates/PLAN.md
skills/team-setup/templates/CLAUDE-sections.md
skills/team-setup/templates/role-notes.md
```
