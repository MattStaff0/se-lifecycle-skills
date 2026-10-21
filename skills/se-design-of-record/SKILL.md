---
name: se-design-of-record
description: Use when designing a use case (UC-<AREA>-<slug>) before implementing it, drafting or revising docs/design/<area>.md, checking a use case against the spec and existing code for ambiguities, verifying a design is complete enough to build from, or adding a /design command to a repository.
---

# SE Design-of-Record

Work with the developer on the design-of-record for one use case area, `docs/design/<area>.md`: how the code will realize its use cases, written before the code and approved by pull request before anyone builds from it. Whatever it leaves open, the coding agent invents. In the course: **week 7** for the proving slice (design merged by pull request), then one area at a time.

## Preconditions (refuse to start when one fails)

1. **The architecture-of-record exists**, with §5.2 (components) and §8 (security and crosscutting concepts). The design takes its file paths, error shape, and conventions from there. If it is missing, stop and go to `se-architecture-of-record`; never assume a stack.
2. **The use case is written**, with its extensions, in `docs/requirements/use-cases.md`.
3. **The use case is settled.** If any step or extension depends on an open `OI-*`, stop and list them. The developer decides whether to get the client's answer first or to narrow what this design covers; a design built on a guess about client policy is a guess with diagrams.
4. **A fresh session**, so the design reads the documents rather than an earlier conversation.

## Course template

The course template (`tcu-cosc-40943/course-templates`, `design/design-of-record.md`) is the authority; this skill adds the workflow around it. Where the two disagree, the template wins, and say so.

- **Find the current copy.** Local clone: `$COURSE_TEMPLATES`, else `~/Desktop/tcu-fall-2026/software-engineering/course-templates`. Run `git -C <clone> fetch` (if fetching is not possible, say when the clone was last updated, `git -C <clone> log -1 --format=%cd`, and continue); if behind, show `git -C <clone> log --oneline HEAD..@{u}` and pull only on a yes. No clone: `gh api -H "Accept: application/vnd.github.raw" repos/tcu-cosc-40943/course-templates/contents/design/design-of-record.md`.
- **One document per area**, `docs/design/<area>.md` (lowercase area code, e.g. `ord.md`). If it exists, revise it in place: the overview, class diagram, and data model are revised; the sequence diagrams, API contract rows, and test rows are appended per use case. Otherwise copy the template there with `cp -n`. Never a per-use-case file.
- **Fill underneath the instructions** and leave them in place; the agent reads them on every later pass. The worked-example rows and diagrams (the Cafeteria Ordering System) are replaced by your own.
- **Delegate vs. keep human** (the template's "Working this document with your agent"). Claude may draft the diagrams from the sketch and the use case, list the existing files a class should extend, fill the contract rows' request and response fields, propose a test row for each extension, draft the rejected alternative for a decision already made, and check that every cited identifier exists. **The developer decides which side of the "would a wrong guess break something" line each detail falls on, and every key decision; a teammate's merged review is the approval.**

Filled-in example: Project Pulse's `docs/design/not.md` (`Washingtonwei/project-pulse`). Read it for the shape, not the answers.

## Standing rules

- **The pinning litmus.** Pin what a wrong guess would break (a requirement, a business rule, a quality attribute, or the contract between frontend and backend); leave variable names, field types, exact JSON, and column lengths to the agent. In a greenfield area, never drop the expected file paths, the API contract, the key decisions, or the test list.
- **No scope nobody asked for.** Every sequence interaction is labeled with the use case step it implements (`(step 7)`, `(3a)`); an interaction with no step is a missing requirement (to the spec and the client) or scope creep (cut it). Watch for admin overrides, retry queues, status fields, and notifications "while we are here".
- **New or reused.** Every class is marked new or reused (with its owning package once code exists), so the agent extends what exists instead of building a parallel service.
- **Code is evidence, never authority.** Designing an area that was coded before it was specified ("design-as-built"): record what the code already does, flag every departure from the confirmed spec, and let the client call each one intended, corrected, or a defect (`OPEN-ISSUES.md`).
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 0: Challenge the use case (report only)

Read the use case, the business rules it cites, the SRS requirements it depends on, the architecture-of-record's §5.2 and §8, and the existing code in the area. List every contradiction, ambiguous term, missing extension, and conflict with the code or with §8, and say where each one belongs: a spec edit, an `OI-*` for the client, or a question for this design. **Propose no design shape yet.** Propose the spec edits; apply them only on the developer's yes, so the problem is firm before the solution is drawn. Most findings are requirement defects, not design questions.

### Step 1: The developer sketches first

Ask the developer (or the pair) to sketch the main success scenario's sequence diagram and the API contract rows by hand, from the use case and the architecture-of-record: about twenty minutes, on paper or in the file. **Wait for the sketch.** If they choose to skip it, say what that costs: the compare step has nothing to compare, and the agent's draft becomes the anchor.

### Step 2: Claude drafts, in plan mode

In plan mode (Shift+Tab until the status bar says plan mode), propose the whole document before writing any of it, following the template's sections: the header (Realizes, Depends on, See, Designed against: the commit of `main` read), overview, class diagram with every class new or reused and, for a greenfield area, the expected file paths, one `###` sequence diagram per use case with `alt` blocks for extensions and every arrow labeled by step, the API contract (caller and who may call it, request, success, the error each extension returns, using §8.2's error shape), key decisions (each naming the rejected alternative; the developer makes the decision), the data model change ("No change" with a reason when there is none; an ER diagram only for more than one new table), reuse and cross-cutting citing §8.2 by name, the test list (`Use case, flow | Level | Asserts`, unit or integration, one row per main flow and every extension, plus an authorization row), changes to the architecture-of-record (or "None"), and open questions. **Wait for the developer to accept the shape.**

### Step 3: Compare, then write

Lay the sketch beside the draft. For each difference, the developer decides which is right and why. What the agent caught goes in. What the developer knew that the agent could not have is context the specification is missing: fix the spec too (or log an `OI-*`). Additions that cite no use case step come out. Then write `docs/design/<area>.md`. If the design changes a component, a responsibility, or a dependency in the architecture-of-record, change that document in the same branch and list each change.

### Step 4: The questions test (the developer runs it)

Claude cannot run this one: the session that drafted the design knows too much. Tell the developer to start a fresh session in plan mode, give it only the use case, the business rules it cites, the architecture-of-record's §8, and the design, and ask:

> List every point you would still have to guess to implement `<UC-ID>`. For each, say what you would guess and whether a wrong guess could violate a requirement. Do not write code.

When they paste the list back, for each item either fix the design or keep the guess with one line saying why a wrong guess breaks no requirement. The design is ready when nothing on the list could break a requirement.

### Step 5: The pull request

If the repository has no `.github/PULL_REQUEST_TEMPLATE.md`, copy it from the course templates (`cp -n`). Draft the description in its shape, at most three lines per section: the use case under Traces to, the challenge findings and spec edits under What changed, the questions-test list with an answer to each item in a collapsed `<details><summary>Questions test</summary>…</details>` block under How it was verified, and under Reviewer focus the decisions most likely to be wrong and any change to the architecture-of-record. Fill **Not verified**. Push and open it only on the developer's yes. A teammate who did not write it reviews and merges it; **no implementation branch starts until it is merged.** If `docs/traceability.md` has §3 (week 8 onward), the row's Design cell points at `design/<area>.md`, and Build becomes `📐 Designed` when the pull request merges.

Then hand off: after the merge, `/clear` and `se-implementation-and-calibration` (`/implement <UC-ID>`) in a fresh session.

## The `/design` command

To give a repository this workflow as a slash command, copy the command in `templates/design-command.md` to `.claude/skills/design/SKILL.md` (the older form, `.claude/commands/design.md`, also works).
