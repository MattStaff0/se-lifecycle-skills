# Slash command: `/design`

Copy the block below into the target repository as `.claude/skills/design/SKILL.md` (the course's current form), or `.claude/commands/design.md` (the older form, which still works). Commit it by pull request, as the team runs it.

**Source:** adapted from Project Pulse's `.claude/commands/design.md` (github.com/Washingtonwei/project-pulse, Apache-2.0), following the course's skill skeleton (AI-Assisted Implementation 4.11: front matter, the argument, preconditions, standing rules, phases that read first, stops and handoff). Changes from Pulse: its area codes, FR families, and subsystems are replaced by the repository's own documents; its `🔬 Problem-validated` state is dropped because the course's traceability Build column has no such state; and the order is the course template's (sketch yourselves first, agent drafts in plan mode, compare, questions test, pull request). Edit the paths if your repository differs.

````markdown
---
description: "Design a use case before code: challenge it, take the developer's sketch, propose the design in plan mode, compare, write docs/design/<area>.md. Stops before any code."
argument-hint: "<UC-ID>"
disable-model-invocation: true
---

# /design: turn a use case into a design-of-record

Take the use case named in `$ARGUMENTS` and produce its design-of-record, `docs/design/<area>.md` (lowercase area code), from the template `design/design-of-record.md` in the course templates. Design is its own reviewed stage and **stops before any code**; `/implement <UC-ID>` builds from it after it merges.

## Preconditions

- `$ARGUMENTS` names a use case in `docs/requirements/use-cases.md`, written with its extensions. If not, ask which one.
- `docs/design/architectural-design.md` exists with §5.2 and §8. If not, stop: design the architecture first.
- No step or extension of the use case depends on an open `OI-*` in `docs/requirements/OPEN-ISSUES.md`. If one does, stop and list them; the developer decides whether to ask the client first.
- A fresh session. If this one carries unrelated work, ask for `/clear`.

## Standing rules

- **The spec is authoritative but not infallible: challenge it.** Ambiguous step, contradiction, a constraint that is stale or not best practice: ask or push back; never silently comply or silently invent. When in doubt, or when you see a better way, shout.
- **Never answer for the client.** A policy question becomes an `OI-*`.
- **Cite, never copy.** The design cites `UC-*`, `BR-*`, and `FR-*` by ID and says only how.
- **Pin what a wrong guess would break**; leave the rest to `/implement`.
- **Every interaction names the use case step it implements.** One that names none is a missing requirement or scope creep.
- **The developer decides** each key decision and which side of the pin-or-derive line a detail falls on.
- Propose commits and the pull request; make them only on a yes.

## Phase 1: Challenge the use case (read only, report only)

Read the use case (Grep its ID; don't read the file whole), its `BR-*` rules, the SRS requirements it depends on, the glossary terms it uses, the architecture-of-record's §5.2 and §8 in full, any existing `docs/design/<area>.md` (revise in place, never a per-use-case file), and the existing code for the area. Built but never designed? Then this is design-as-built: the code is evidence, never authority; flag every departure from the spec.

Report every contradiction, ambiguous term, missing extension, and conflict with the code or §8, and where each belongs: a spec edit, an `OI-*`, or a design question. Propose the spec edits and apply them only on a yes. **Propose no design shape in this phase.**

## Phase 2: The developer's sketch

Ask the developer to sketch the main success scenario's sequence and the API contract rows by hand (about twenty minutes). **Stop until they share it,** or say skip (and say what skipping costs).

## Phase 3: Propose the design (plan mode)

If not already in plan mode, ask the developer to enter it. Propose the whole document, following the template's sections and instructions: header (Realizes, Depends on, See, Designed against), overview, class diagram (new or reused; expected file paths for a greenfield area), a `###` sequence diagram per use case with `alt` blocks and step labels, the API contract (caller and scoping, request, success, the error per extension in §8.2's shape), key decisions with the rejected alternative, the data-model change (or "No change" and why), reuse citing §8.2, the test list (`Use case, flow | Level | Asserts`, every extension plus authorization), changes to the architecture-of-record (or "None"), and open questions. **Edit nothing. Stop until the developer accepts the shape.**

## Phase 4: Compare, then write

For each difference from the sketch, the developer decides which is right. What the agent caught goes in; what the developer knew that the agent could not becomes a spec fix or an `OI-*`; additions with no step come out. Then write `docs/design/<area>.md` (copy the template first if the file does not exist), leaving its instructions in place. Apply any approved change to `docs/design/architectural-design.md` in the same branch and list it.

## Phase 5: Questions test and pull request

Stop and tell the developer to run the questions test in a **fresh** session in plan mode, with only the use case, its business rules, §8 of the architecture-of-record, and this design:

> List every point you would still have to guess to implement `<UC-ID>`. For each, say what you would guess and whether a wrong guess could violate a requirement. Do not write code.

For each item they paste back: fix the design, or keep the guess with a one-line reason. Then draft the pull request description from `.github/PULL_REQUEST_TEMPLATE.md` (copy it from the course templates if the repository lacks it) (three lines per section; the questions-test list in a collapsed `<details>` block under How it was verified; Not verified filled). Push and open it only on a yes. A teammate who did not write it reviews and merges it. If `docs/traceability.md` has its §3, set the row's Design cell; Build reads `📐 Designed` once the pull request merges.

Hand off: after it merges, `/clear`, then `/implement <UC-ID>`.
````
