# se-lifecycle-skills

Claude Code skills that encode a spec-driven software engineering lifecycle — from team charter through requirements, architecture, design, and implementation — so an AI agent works inside the same process a human team would.

## Skills

| Skill | Stage | What it scaffolds |
|---|---|---|
| `se-charter-and-governance` | Charter | `AGENTS.md` / `CLAUDE.md`, `.claude/settings.json` deny rules, team contract |
| `se-requirements-foundation` | Requirements | Glossary, vision & scope, business rules, open issues |
| `se-use-cases-and-srs` | Requirements | Use cases, EARS-style SRS, traceability matrix |
| `se-architecture-of-record` | Architecture | arc42 architecture doc, C4 diagrams, ADRs |
| `se-design-of-record` | Design | Per-area design docs, `/design` command |
| `se-implementation-and-calibration` | Build | Estimation, build-context issues, trunk/leaf implementation, review triage |
| `se-new-repo-orchestrator` | Orchestrator | Walks a greenfield repo through the lifecycle |
| `se-slice-orchestrator` | Orchestrator | Walks a vertical slice through an existing repo |

## Install

**As a plugin (anyone):**

```
/plugin marketplace add MattStaff0/se-lifecycle-skills
/plugin install se-lifecycle-skills@se-lifecycle-skills
```

**For local development (live edits):** symlink each skill into `~/.claude/skills/`:

```bash
for d in skills/*/; do ln -s "$PWD/$d" ~/.claude/skills/"$(basename "$d")"; done
```

Use one method or the other, not both — otherwise each skill loads twice.

## The workflow

How to use each skill, with example prompts and where each one stops for you: [`USAGE.md`](USAGE.md).

The week-by-week map of the course, which skill runs each step, and every point where a human decides: [`skills/se-new-repo-orchestrator/references/COURSE-LIFECYCLE.md`](skills/se-new-repo-orchestrator/references/COURSE-LIFECYCLE.md). Not sure where you are? Ask Claude to run `se-new-repo-orchestrator` (whole project) or `se-slice-orchestrator` (one use case); each diagnoses the repository and stops for you to pick.

## Where the templates come from

- **Course pages** are the source for the Napkin, the charter's contents, the team contract, the `.claude/settings.json` deny rules, the estimating and calibration rules, and the code-review checklist ([course site](https://washingtonwei.github.io/cosc-40943/)). Each file names the page it came from; where a page has changed since, the page wins.
- **Course document templates** (glossary, vision and scope, business rules, open issues, use cases, SRS, traceability, architecture-of-record, design-of-record, PR template) are **not copied into this repo**. The skills read the current version from [`tcu-cosc-40943/course-templates`](https://github.com/tcu-cosc-40943/course-templates), from a local clone (pulled first) or straight from GitHub, because the templates are corrected during the semester.
- **Agent setup** (the `AGENTS.md` boilerplate and the `/design`, `/implement`, `/estimate`, `/close-use-case` commands, the build-context issue, the review checklist, and the lifecycle map) is adapted from [Project Pulse](https://github.com/Washingtonwei/project-pulse), the course's running example. Each file states which Pulse file it came from and what changed.

## Attribution

Portions of this repository are adapted from Project Pulse by Bingyang Wei, licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). The adapted files were modified: framework- and module-specific details were replaced with pointers to the target repository's own architecture-of-record, and course-specific steps were added.
