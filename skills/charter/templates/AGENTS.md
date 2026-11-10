<!-- AGENTS.md boilerplate for the spec-driven cycle. The agent reads this file in every session, so
     replace each _[bracket]_ with the project's fact and delete the bracket. Leave out a section you
     cannot fill yet (run commands before code exists) and add it the day it becomes true; the same for
     a line that points at a file that does not exist yet. Write it thin: it grows every time the agent
     gets something wrong that nobody told it. Pair it with a CLAUDE.md whose first line is exactly
     @AGENTS.md. The layering and the spec-first workflow are adapted from Project Pulse's CLAUDE.md
     files (github.com/Washingtonwei/project-pulse, Apache-2.0). -->

# AGENTS.md

## What this is

_[Three sentences: what the product is, who it is for, and who asked for it.]_

**Project types:** _[application | data | ML; list each that applies. See the `lifecycle` skill's `project-types.md`.]_

**Profile:** _[none, or a profile from the `lifecycle` skill's `references/profiles/` (for example `cosc-40943`), which can change the templates, the calendar, and a few rules]_

**Built spec-first:** the documents below are the contract. Don't design ad hoc: start from the spec, build from it, and trace the work back.

## Documents

| Document | Path | Template source |
|---|---|---|
| Glossary | `docs/requirements/glossary.md` | _[bundled, or the team's or course's template]_ |
| Vision and scope | `docs/requirements/vision-and-scope.md` | |
| Open issues | `docs/requirements/OPEN-ISSUES.md` | |
| Business rules | `docs/requirements/rules.md` | |
| Slice contracts | `docs/requirements/use-cases.md`, `data-contracts.md`, `model-contracts.md` _[only the types that apply]_ | |
| Requirements | `docs/requirements/requirements.md` | |
| Traceability | `docs/traceability.md` | |
| Architecture-of-record | `docs/design/architecture.md` | |
| Designs | `docs/design/<area>.md`, one per area | |
| Experiments | `docs/experiments/<slug>.md` | |

_[Change a path when the repository already has a convention (an `adr/` folder, a wiki); the skills read this table.]_

## How to run it

_[The exact commands, in order, that take a clean machine to a running system.]_

## How to run the tests

_[All tests; one test; data tests; evaluation; and what passing means.]_

_[The gate: the one command, the same the pull request runs, that must pass before a change is done (the `static-analysis` skill). Passing tests alone is not passing the gate.]_

## Conventions

The binding conventions live once, in the crosscutting concepts section of the architecture-of-record. Each line here is a reminder that links to it.

- _[Only what the team has actually agreed: naming, layout, error handling, time, validation, logging.]_
- _[Where existing code disagrees with a convention, say so and name the file to copy instead.]_

**Logging.** Log identifiers and the failure reason, never content: no secrets, tokens, or passwords, and for a person their ID, never their email or other personal data. Never log a whole object; its default string form prints every field. Every exception handler that does not rethrow logs the exception.

## The workflow the agent follows

- Work from an issue, on a branch; never push to the main branch. Every change is a pull request (at most three lines per description section, never restating the diff).
- **The slice is the contract.** A use case, data contract, or model contract gives what to build and every failure path to test. A slice is designed (design-of-record, merged by pull request) before it is built, in a fresh session.
- **The glossary fixes vocabulary.** Use the defined term in identifiers, column names, and UI text, never a synonym.
- **Challenge, never comply silently.** When a step is ambiguous, an assumption breaks against the code, or requirements contradict, ask or push back. When in doubt, or when you see a better way, shout.
- **Never answer for a stakeholder.** A question only a stakeholder can answer goes to the open-issues file.
- **Cite, never copy.** Refer to slices, rules, requirements, and decisions by ID and path.
- **Pin or derive.** If guessing a detail wrong would violate a requirement, the spec or design pins it; otherwise derive it from the conventions.
- **The gate is the analyzers, and you answer to it** _[once it exists; cite the architecture-of-record's Static analysis subsection]_. Every suppression carries a reason a reviewer can check; a baseline only shrinks; never weaken a check, suppress a finding, or add a baseline entry to make the build pass without saying so in the pull request.
- **Code and notebooks are evidence, never authority.** Behavior read back from unspecified code, or a result from an exploratory notebook, is not a requirement until someone decides it is.

### Running more than one agent

- One agent per sub-issue, each in its own worktree and branch (`claude --worktree <name>`), each ending in its own pull request.
- Fan out only after the shared code the leaves depend on is merged.
- Never split one slice's layers across agents.
- No more agents running than pull requests someone will review that day.

## What the agent must not do

- Never add a dependency until a person confirms it exists and is the one intended.
- Never remove a `Co-Authored-By` trailer from a commit.
- Never read secrets. `.claude/settings.json` denies `.env`, `secrets/`, `*.pem`, and `*.key`; secrets live in _[where]_, never in git.
- Never push to the main branch, force-push, merge a pull request, or delete a repository.
- Never put production data, credentials, or personal data in a prompt, a fixture, or a notebook output that gets committed.
- _[Every rule added after the agent got the same thing wrong twice.]_

## Directory charters

_[Only once the directory exists. Stack-specific rules live next to the code (`backend/AGENTS.md`, `pipelines/AGENTS.md`, `models/AGENTS.md`, …), each with its own one-line `CLAUDE.md` containing `@AGENTS.md`. Each names the reference implementation to read before building something new, the conventions with a short sketch, and any known drift.]_
