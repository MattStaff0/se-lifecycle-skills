---
name: se-charter-and-governance
description: Use when onboarding an AI coding agent into a repository, creating or revising AGENTS.md / CLAUDE.md (root or per-directory), configuring .claude/settings.json deny rules for secrets, turning an agent mistake caused by missing context into a durable charter rule, or drafting docs/team-contract.md.
---

# SE Charter and Governance

Set up the durable half of the agent's context: the charter it reads every session (`AGENTS.md`, imported by a one-line `CLAUDE.md`), the deny rules Claude Code enforces (`.claude/settings.json`), and the team contract (`docs/team-contract.md`). In the course: team contract at **Checkpoint 0** (week 2); charter written thin in week 2; `.claude/settings.json` and the charter's must-not-do lines checked at **Checkpoint 2**.

## Standing rules

- **The repository is the only memory.** The agent has amnesia between sessions; a decision not written in the repo does not exist for it.
- **Write thin, grow from scars.** Start with the six things below. Each time the agent gets something wrong because nobody told it, add the rule that would have prevented it, by a small pull request. A vague rule ("write secure queries") does nothing; write a checkable one ("every query in a team-scoped service filters by the caller's `teamId` from `UserUtils`, never one from the request body").
- **Conventions live next to the code.** Project-wide rules in the root `AGENTS.md`; stack rules in `backend/AGENTS.md`, `frontend/AGENTS.md`, created when those directories exist.
- **Enforce secrets by rule, not prose.** "Never read `.env`" in a charter is advice; a `Read` deny rule is enforced by Claude Code.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it. Ask for the facts only they have; never invent run commands, conventions, or team agreements.
- **Git:** work on a branch; propose each commit and pull request and make it only on a yes. Never remove a `Co-Authored-By` trailer.

## Workflow

### Step 1: Gather the facts that exist today

Ask for, or read from the repo: the product in three sentences and the client; the commands that exist yet (run, test); the directory layout, with every domain abbreviation spelled out (`ram/` = Requirements Authoring and Management) so a search for the full name finds the code; conventions the team has actually agreed on. In week 2 most of this does not exist yet: leave those sections out rather than writing placeholders the agent will read as fact.

### Step 2: Write `AGENTS.md` and `CLAUDE.md`

Draft the root `AGENTS.md` from `templates/AGENTS-boilerplate.md`, and a `CLAUDE.md` whose **first line is exactly `@AGENTS.md`**: an import, never a sentence like "See AGENTS.md" (with a sentence, Claude Code reads `CLAUDE.md` alone and sees the charter only if it decides to open it). Match the filename's capitals: `@agents.md` resolves on macOS and Windows and fails on Linux and in CI. Anything Claude-specific goes under the import line.

The charter carries the six things the course lists: what the product is, how to run it, how to run the tests, the agreed conventions, the workflow the agent must follow, and what it must not do. The must-not-do list includes the **dependency rule** and **never removing the `Co-Authored-By` trailer** (both checked at Checkpoint 2). If the team runs more than one agent at a time, the workflow section has the **four parallel-agent lines**.

Then have the developer verify the import: run `/context` in a new session and check that `CLAUDE.md` and `AGENTS.md` appear under Memory files. **Wait** for them to confirm before moving on.

### Step 3: Write `.claude/settings.json` and test it

Draft `.claude/settings.json` from `templates/claude-settings.json` (the course's file from Agent Setup §10). If secrets live somewhere else, add that path. Add `.env` to `.gitignore`. It merges by pull request, so the team reviews what the agent may not do.

Then test the lock with the developer, using a dummy `.env` with no real secret in it: ask to read it with the Read tool, and to `cat .env`; both must be refused. **Wait** for both refusals; if either read succeeds, fix the rule before continuing. Two limits to state plainly: the rules load from the `.claude/` folder of the directory Claude Code is launched in (launch it from the repo root); and they do not stop `grep -r`, or a script that opens the file, so OS-level enforcement needs Claude Code's sandbox. The course's user-level deny rules (Agent Setup §9, added through `/permissions`) are the developer's own settings: point to them, don't change them.

### Step 4: The team contract (week 2, Checkpoint 0)

Copy `templates/team-contract-template.md` (the course's template, verbatim) to `docs/team-contract.md`, or fetch the current one from the course site as its header says. Coach the team through it the course's way: ten minutes, together, out loud; **fix the meeting time first**; write what you will actually do. Then explain the signing rule: each member adds their own signature line **in their own commit**, from their own account; one person typing everyone's names does not count. For clause 7, offer the Professionalism module's four steps (Observe the behavior, Analyze its impact, Investigate before assuming, Fix with an owner and a date). The team writes the clauses; Claude never fills in a team agreement.

### Step 5: Turn a scar into a rule

When the agent gets something wrong for lack of context: write the rule that would have prevented it, as a checkable line in the charter that names the exemplar file to copy, and propose it as its own small pull request. If the rule applies to code already merged, open an issue for that code rather than widening the pull request.

## Gotchas

- **Copying precedent over rules.** When older code violates a new convention, the agent copies the nearest example. Say in the charter that the older files disagree, and name the canonical file to copy.
- **A placeholder is read as a fact.** An unfilled `[run command]` in `AGENTS.md` is in the agent's context every session. Leave a section out until it is true.
