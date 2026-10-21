<!-- Source: the six things a charter carries, from the course's "Onboarding the AI teammate"
     (modules/ai-augmented-team, 4.12); the must-not-do lines and the four parallel-agent lines from
     Agent Setup section 10 and the Checkpoint 2 checklist; the logging rule from AI-Assisted
     Implementation 4.9; the layering and the spec-driven workflow from Project Pulse's root
     CLAUDE.md (github.com/Washingtonwei/project-pulse, Apache-2.0).

     This boilerplate is NOT a course template: the agent reads this file in every session, so
     replace each _[bracket]_ with your project's fact and delete the bracket. Leave out a section
     you cannot fill yet (e.g. run commands before any code exists) and add it the day it becomes
     true. The same goes for a sentence that points at a file that does not exist yet (the architecture-of-record, its §8.2, /design, /implement, the PR template): add it when the file lands. Write it thin; it grows every time the agent gets something wrong that you never told it.
     Pair it with a CLAUDE.md whose first line is exactly: @AGENTS.md -->

# AGENTS.md

## What this is

_[Three sentences: what the product is, who the client is, who uses it.]_ Built spec-first: the requirements in `docs/requirements/` are the contract, and the architecture-of-record is `docs/design/architectural-design.md`.

## How to run it

_[The exact commands, in order, that take a clean machine to a running app, and the URL where it is ready.]_

## How to run the tests

_[The command for all tests, for one test class, and what passing means.]_

## Conventions

The binding conventions live once, in §8.2 of `docs/design/architectural-design.md`. Each line here is a reminder that links to its subsection there.

- _[Naming, formatting, where things go: only what the team has actually agreed on.]_
- _[One line per §8.2 concept once it is written: error handling, time, API shape, validation, logging, …]_
- _[Where existing code disagrees with a convention, say so and name the file to copy instead.]_

**Logging.** Log identifiers, never content: no secrets, tokens, or passwords, and for a person their ID, never their email, scores, or comments. Never log a whole object; its default string form (`toString()` in Java) prints every field. Every `catch` that does not rethrow logs the exception. _[Your logger idiom and levels, per §8.2.]_

## The workflow the agent follows

- Work from an issue. One branch per sub-issue, named _[convention from team contract clause 5]_. Never push to `main`; every change is a pull request, described with `.github/PULL_REQUEST_TEMPLATE.md` (at most three lines per section, never restate the diff).
- **The spec is the contract.** A use case gives what to build and the flows to test, its main flow and every extension. Don't design features ad hoc: `/design <UC-ID>` produces the design-of-record (`docs/design/<area>.md`), it merges by pull request, then `/implement <UC-ID>` builds from it in a fresh session.
- **The glossary fixes vocabulary.** Use the term defined in `docs/requirements/project-glossary.md` in code identifiers and UI text, never a synonym.
- **Challenge, never comply silently.** The spec is authoritative but not infallible. When a step is ambiguous, an assumption breaks against the code, or requirements contradict, ask or push back; don't silently comply or silently invent. When in doubt, or when you see a better way, shout.
- **Never answer for the client.** A question only the client can answer goes to `docs/requirements/OPEN-ISSUES.md`.
- **Cite, never copy.** Refer to `UC-*`, `BR-*`, `FR-*`, quality attributes, and `KD-*` by ID and path.
- **Pin or derive.** If guessing a detail wrong would violate a requirement, the spec or design pins it; otherwise derive it from the conventions.
- **Code is evidence, never authority.** Behavior read back from code nobody specified is not a requirement until the client calls it intended.

### Running more than one agent

- One agent per sub-issue, each in its own worktree and branch (`claude --worktree <name>`), each ending in its own pull request.
- Fan out only after the shared code the leaves depend on is merged.
- Never split one use case's layers across agents.
- No more agents running than pull requests you will review that day.

## What the agent must not do

- Never add a dependency until a person confirms it exists and is the one we meant.
- Never remove the `Co-Authored-By` trailer from a commit.
- Never read secrets. `.claude/settings.json` denies `.env`, `secrets/`, `*.pem`, and `*.key`; secrets live in _[where]_, never in git.
- Never push to `main`, force-push, merge a pull request, or delete a repository.
- _[Every rule the team adds after a review caught the agent doing something twice.]_

## Directory charters

_[Only once the directory exists. Stack-specific rules live next to the code: `backend/AGENTS.md`, `frontend/AGENTS.md`, `docs/AGENTS.md`, each with its own one-line `CLAUDE.md` containing `@AGENTS.md`. Each names the reference implementation to read end to end before building a new feature, the conventions with a short code sketch, and any known drift, e.g. "some pages use verb-first names like `AddActivityForm.vue`; new pages are entity-first, like `ActivityAddForm.vue`".]_
