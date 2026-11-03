# How to use the `sdd` skills

Each skill copies a template, drafts only what an agent should draft, and stops at every decision that's yours or a stakeholder's. Installed as a plugin, they're called `/sdd:<skill>`; you can also just describe what you're doing and Claude picks the matching one.

## Start here

| Say | Skill | What happens |
|---|---|---|
| "Where are we in the lifecycle?" | `lifecycle` | Reads `AGENTS.md` and the repo, names the step you're on, what the next one needs, and the gap. Stops for you to pick. |
| "Where is `DP-FIN-daily-revenue`?" | `slice` | Finds that one slice's stage (challenge, design, build-context, build, close) and its gate. |

When a step finishes, ask again; it re-reads the repo instead of guessing.

## Habits that make it work

- **Fresh session per stage:** `/clear` between design, implementation, and review, never in the middle of one.
- **Plan mode** (Shift+Tab) for designing and anything multi-step.
- **"Ask the stakeholder"** is always a valid answer. It becomes an `OI-*` in the open-issues file; Claude never fills in a policy, number, or definition itself.
- **Yours first:** your napkin before the agent's, your design sketch before its draft, your review before its review.

## The skills, in cycle order

### `lifecycle`: start, set up, pick the proving slice
**Say:** "Run the Napkin for this project" · "Set up docs/" · "Help us pick the proving slice"
**It stops for you:** your own napkin first; what to copy (it never overwrites); the pick.

### `charter`: the agent's durable memory
**Say:** "Set up the agent charter" · "Add the secret deny rules" · "Draft the team contract" · "The agent did X twice, make it a rule"
**You get:** `AGENTS.md` (with the project types and the Documents table every other skill reads), `CLAUDE.md` (`@AGENTS.md`), `.claude/settings.json`, `docs/team-contract.md`.
**It stops for you:** every team agreement; the `/context` check; the `.env` refusal test.

### `discovery`: the stakeholders' world, in their words
**Say:** "Prep me for the stakeholder meeting" · "File the meeting notes" · "Draft the glossary" · "Work on vision and scope" · "Catalog the business rules"
**You get:** an interview record per conversation, the glossary (including metric definitions), vision and scope, open issues, rules.
**It stops for you:** which questions are worth their time; every number, baseline, and definition (or it's an `OI-*`); the MVP line.

### `specification`: contracts precise enough to build and test from
**Say:** "Build the slice list" · "Write `UC-…` / `DP-…` / `MD-…` in full" · "Review this contract" · "Draft the requirements" · "Run the traceability checks"
**You get:** use cases (features), data contracts (pipelines: grain, keys, freshness, quality checks, failure paths), model contracts (decision, baseline, error costs, split, failure slices, fallback), requirements with quantified quality attributes, traceability.
**It stops for you:** stakeholders review the slice list before any detail; priorities; you review each contract alone first; every threshold.

### `architecture`: one map, shaped by ranked quality attributes
**Say:** "Start the architecture-of-record" · "The agent wants Kafka, check it"
**You get:** the architecture document: context and container diagrams, components, security, crosscutting conventions (including data quality, lineage, and reproducibility for data and ML), key decisions.
**It stops for you:** the ranking of the significant requirements and every key decision.

### `design`: the last document before the code
**Say:** "Design `UC-…`" · "Run an experiment on whether …"
**You get:** `docs/design/<area>.md` (flows labeled by contract step, the contract, decisions, the data or ML specifics, the test list), or an experiment record.
**It stops for you:** challenge findings; **your sketch before its draft**; accepting the shape; the comparison; you run the questions test in a fresh session; a teammate merges.
**It refuses** without an architecture-of-record, or while the slice depends on open `OI-*` items.

### `build`: estimate, implement, review, close
**Say:** "Coarse estimate for `…`" · "Write the build-context issue" · "Refined estimate" · "Implement `…`" · "Review PR #N against `…`" · "Close `…`"
**You get:** estimate tables with the hours blank, a build-context issue that cites and never copies, trunk-and-leaf commits, an honest traceability row, calibration ratios.
**It stops for you:** every hour; the trunk pause ("can you say what changed and why without asking the agent?"); every new dependency; pushing; the retrospective.

### `slice`: one slice, start to close
The "where is this slice?" skill; it hands each stage to `design` or `build`.

## Sizing it to the work

A bug fix skips design. A small change amends one contract and one test row. An experiment gets a time-box and a written result, and its code never ships. A small project keeps three things: the charter, contracts with their failure paths, and a test list per slice. See `skills/lifecycle/references/cycle.md`.

## Using it on an existing codebase

Start with `lifecycle`. The recovered documents are drafted from the code, but **the code is evidence, never authority**: a stakeholder calls each recovered behavior intended, corrected, or a defect. Designs are written only where work lands, "as built".

## Team-wide commands (optional)

Teammates without the plugin can share the workflow through repo-local commands, committed by pull request:

| Command | Copy from |
|---|---|
| `/design` | `skills/design/templates/design-command.md` |
| `/estimate`, `/implement`, `/close` | `skills/build/templates/commands.md` |
| Slice issue template | `skills/build/templates/build-context-issue.md` → `.github/ISSUE_TEMPLATE/slice.md` |
| PR template | `skills/build/templates/pull-request.md` → `.github/pull_request_template.md` |

## Templates

Every skill uses the project's own template when `AGENTS.md`'s Documents table names one (a team folder, a course repository), and otherwise the bundled template in its `templates/` folder. A project with a profile (such as `cosc-40943`) gets that profile's templates and calendar.

## If something's off

- **A skill didn't trigger:** call it by name (`/sdd:<skill>`).
- **Claude wrote a number, policy, or definition you never gave it:** that's a bug. Tell it "that's an `OI-*`", then fix the skill.
- **The charter isn't loading:** `CLAUDE.md`'s first line must be exactly `@AGENTS.md`; check with `/context`.
- **Updates aren't arriving:** `/plugin marketplace update spec-driven-dev`; on a private repo, run `gh auth setup-git` once.
