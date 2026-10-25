---
name: charter
description: Use when onboarding an AI coding agent into a repository, creating or revising AGENTS.md / CLAUDE.md (root or per-directory), configuring .claude/settings.json deny rules for secrets, turning an agent mistake caused by missing context into a durable rule, or drafting a team contract.
---

# Charter

Set up the durable half of the agent's context: the charter it reads every session (`AGENTS.md`, imported by a one-line `CLAUDE.md`), the deny rules Claude Code enforces (`.claude/settings.json`), and, for a team, the team contract. It is step 1 of the cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder), and the charter is revisited every time the agent gets something wrong twice.

## Standing rules

- **The repository is the only memory.** The agent has amnesia between sessions; a decision not written in the repo does not exist for it.
- **Write thin, grow from scars.** Start with the six things below. Each time the agent gets something wrong because nobody told it, add the rule that would have prevented it. A vague rule ("write secure queries") does nothing; write a checkable one, and name the exemplar file to copy.
- **Conventions live next to the code.** Project-wide rules in the root `AGENTS.md`; stack rules in `backend/AGENTS.md`, `pipelines/AGENTS.md`, and so on, created when those directories exist.
- **Enforce secrets by rule, not prose.** "Never read `.env`" in a charter is advice; a `Read` deny rule is enforced by Claude Code.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it. Ask for the facts only they have; never invent run commands, conventions, or team agreements.
- **Git:** work on a branch; propose each commit and pull request, and make them only on a yes. Never remove a `Co-Authored-By` trailer.

## Workflow

### Step 1: Gather the facts that exist today

Ask for, or read from the repo: what the product is and who it is for; which **project types** apply (application, data, ML; see `../lifecycle/references/project-types.md`); the commands that exist yet (run, test, data tests, evaluation); the directory layout, with every abbreviation spelled out so a search for the full name finds the code; conventions the team has actually agreed on; and any existing documentation convention (an `adr/` folder, a wiki, a team template folder, a course's template repository). On a brand-new project most of this does not exist yet: leave those sections out rather than writing placeholders the agent will read as fact.

### Step 2: `AGENTS.md` and `CLAUDE.md`

Draft the root `AGENTS.md` from `templates/AGENTS.md`, and a `CLAUDE.md` whose **first line is exactly `@AGENTS.md`**: an import, never a sentence like "See AGENTS.md" (with a sentence, Claude Code reads `CLAUDE.md` alone and opens the charter only if it decides to). Match the filename's capitals: `@agents.md` fails on Linux and in CI. Anything Claude-specific goes under the import line.

Fill the **Documents** table: where each document lives and which template it comes from (bundled by default; a team's or course's templates if the project has them). Every other skill reads that table, so this is how a project keeps its own conventions.

The charter carries six things: what the product is, how to run it, how to run the tests, the agreed conventions, the workflow the agent follows, and what it must not do (including the dependency rule and never removing `Co-Authored-By`). If more than one agent runs at once, the workflow section has the four parallel-agent lines.

Then have the developer verify the import: `/context` in a new session should list `CLAUDE.md` and `AGENTS.md` under Memory files. **Wait** for them to confirm.

### Step 3: `.claude/settings.json`, and test it

Draft `.claude/settings.json` from `templates/settings.json`; if secrets live somewhere else, add that path. Add `.env` to `.gitignore`. It merges by pull request, so the team reviews what the agent may not do.

Test the lock with the developer, using a dummy `.env` holding no real secret: ask to read it with the Read tool, and to `cat .env`; both must be refused. **Wait** for both refusals; if either read succeeds, fix the rule before continuing. State the limits plainly: the rules load from the `.claude/` folder of the directory Claude Code is launched in (launch it from the repo root), and they do not stop `grep -r` or a script that opens the file, so OS-level enforcement needs Claude Code's sandbox. Personal deny rules (added with `/permissions`) are the developer's own settings: suggest them, never change them.

### Step 4: The team contract (teams only)

Copy `templates/team-contract.md` to `docs/team-contract.md` (or the team's own template, if the Documents table names one). Coach the team through it: together, out loud, **the meeting time first**, then only what they will actually do. Each member signs in their own commit. The team writes every clause; Claude never fills in a team agreement.

### Step 5: Turn a scar into a rule

When the agent gets something wrong for lack of context, write the rule that would have prevented it as a checkable line that names the exemplar file, and propose it as its own small pull request. If it applies to code already merged, open an issue for that code rather than widening the pull request.

## Gotchas

- **Copying precedent over rules.** When older code violates a new convention, the agent copies the nearest example. Say in the charter that the older files disagree, and name the file to copy.
- **A placeholder is read as a fact.** An unfilled `[run command]` is in the agent's context every session. Leave a section out until it is true.
