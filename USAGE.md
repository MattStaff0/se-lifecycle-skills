# How to use the SE lifecycle skills

Eight Claude Code skills that walk you through COSC 40943's spec-driven workflow. Each one copies the official course template, drafts only what the template lets an agent draft, and stops at every decision that's yours or the client's. The week-by-week map and every human gate are in [`COURSE-LIFECYCLE.md`](skills/se-new-repo-orchestrator/references/COURSE-LIFECYCLE.md); this file is the how-to.

## Start here

Not sure what's next? Open Claude Code in your team repository and say one of these:

| Say | Skill | What happens |
|---|---|---|
| "Where are we in the lifecycle?" | `se-new-repo-orchestrator` | Reads the repo and today's date, then names the stage you're in, what the next checkpoint checks, and what's missing. Stops for you to pick. |
| "Where is `UC-ORD-place-order`?" | `se-slice-orchestrator` | Finds that one use case's stage (challenge, design, build-context, build, close) and its gate. Stops for you to pick. |

When a stage finishes, ask again; the orchestrator re-reads the repo instead of guessing.

## Using a skill

- **By description:** just say what you're doing ("help me prep for the client meeting") and Claude picks the matching skill.
- **By name:** type `/se-requirements-foundation` (or any skill name) to force it.
- **Fresh session per stage.** Design, implementation, and review each start clean: run `/clear` between them, never in the middle of one.
- **Plan mode** (Shift+Tab until the status bar says plan mode) for designing and anything multi-step: Claude proposes before it writes.
- **"Ask the client"** is always a valid answer. Claude turns it into an `OI-*` in `docs/requirements/OPEN-ISSUES.md` and never fills in a client policy or number itself.

## The skills, in course order

### `se-charter-and-governance` (week 2, Checkpoints 0 and 2)
**Say:** "Set up our agent charter" · "Draft the team contract" · "Add the secret deny rules" · "The agent did X wrong twice, make it a rule"
**You get:** `AGENTS.md` and a one-line `CLAUDE.md` (`@AGENTS.md`), `.claude/settings.json` (the course's deny rules), and `docs/team-contract.md` (the course's seven clauses).
**It stops for you:** every team agreement in the contract (meeting time first); the `/context` check that the charter loads; the `.env` refusal test.

### `se-requirements-foundation` (weeks 3–4, Checkpoint 1)
**Say:** "Prep me for the client meeting" · "File the meeting notes" · "Draft the glossary" · "Work on vision and scope" · "Catalog the business rules"
**You get:** a copy of the interview guide for each meeting, plus the glossary, vision and scope, open issues, and business rules, filled in underneath the template instructions.
**It stops for you:** it interviews you a few questions at a time; you choose which questions are worth the client's hour; MVP scope; every number and baseline (or it becomes an `OI-*`).

### `se-use-cases-and-srs` (weeks 4–5, Checkpoint 1)
**Say:** "Build the use case list" · "Write `UC-…` in full" · "Review this use case" · "Draft the SRS" · "Update traceability"
**You get:** the use case list with Larman's Boss/EBP/Size tests as coaching, full use cases in the template's field order, the SRS, and `docs/traceability.md` §1–2 (and §3 rows from week 8).
**It stops for you:** the client reviews the use case list before any flow is written; you set priorities; you review each use case alone before the agent does; you supply every §9 threshold.

### `se-architecture-of-record` (week 6, Checkpoint 1)
**Say:** "Start the architecture-of-record" · "Draft §8 crosscutting concepts" · "The agent wants microservices, check it"
**You get:** `docs/design/architectural-design.md` (C4 context and container diagrams, the §5.2 component table, §8.1 security, §8.2 conventions, `KD-deployment-shape`), with sections that aren't due yet left as they are.
**It stops for you:** you rank the §9.1 requirements and make every `KD-*` decision; Claude only proposes candidates and drafts the rejected alternatives.

### `se-design-of-record` (week 7, then one area at a time)
**Say:** "Design `UC-…`" (or `/design UC-…` once it's installed in the repo)
**You get:** `docs/design/<area>.md` with sequence diagrams labeled by use case step, the API contract, key decisions, and the test list, plus a PR description that has the questions test in it.
**It stops for you:** the challenge findings; **your 20-minute sketch before Claude drafts anything**; accepting the shape; comparing the two; you run the questions test in a fresh session; a teammate merges the PR.
**It refuses** if there's no architecture-of-record, or if the use case still depends on open `OI-*` items.

### `se-implementation-and-calibration` (week 8 onward, Checkpoints 2–3)
**Say:** "Coarse estimate for `UC-…`" · "Write the build-context issue" · `/estimate UC-… refined` · `/implement UC-…` · "Review this PR against the contract" · `/close-use-case UC-…`
**You get:** estimate tables with the hours left blank, a build-context issue that cites and never copies, trunk-and-leaf commits, an honest traceability row, and calibration ratios.
**It stops for you:** **every hour in every estimate is yours**; the trunk pause ("can you say what changed and why without asking the agent?"); every new dependency; pushing and opening the PR; the retrospective answers.

### `se-new-repo-orchestrator` and `se-slice-orchestrator`
These are the "where am I" skills (see **Start here**). The new-repo one also runs the Napkin: you write yours silently, the team reconciles, and only then does Claude draft its napkin and diff it against yours. It also helps you pick the proving slice.

## Repo slash commands (optional, for the whole team)

The course suggests committing workflow commands to the team repo by pull request. Copy each one to `.claude/skills/<name>/SKILL.md`:

| Command | Copy from |
|---|---|
| `/design` | `skills/se-design-of-record/templates/design-command.md` |
| `/estimate`, `/implement`, `/close-use-case` | `skills/se-implementation-and-calibration/templates/claude-commands.md` |
| `/new-repo-setup` (copies the course templates into `docs/` without overwriting) | `skills/se-new-repo-orchestrator/references/CLAUDE-COMMAND.md` |
| Use-case issue template | `skills/se-implementation-and-calibration/templates/build-context-issue.md` → `.github/ISSUE_TEMPLATE/use-case.md` |

## Setup and upkeep

- **Installed as:** symlinks from `~/.claude/skills/se-*` to this repo's `skills/` folders. Edit a skill here and the next session uses it.
- **Course templates:** the skills read them from `$COURSE_TEMPLATES` if it's set, otherwise `~/Desktop/tcu-fall-2026/software-engineering/course-templates`. They fetch first and only pull with your yes. With no clone, they read the templates from GitHub.
- **Course pages win.** The team contract, deny rules, Napkin, estimating rules, and review checklist come from the course site; if the professor updates a page, the page wins over the copy here.
- **Changing a skill:** branch, edit, open a PR against `main`.

## If something's off

- **A skill didn't trigger:** call it by name (`/se-…`).
- **"Template not found":** check the clone path above, or set `COURSE_TEMPLATES`.
- **Claude wrote a number or policy you never gave it:** that's a bug in the skill. Tell it "that's an `OI-*`", then fix the skill so it doesn't happen again.
- **The charter isn't loading:** `CLAUDE.md`'s first line must be exactly `@AGENTS.md`; check with `/context`.
