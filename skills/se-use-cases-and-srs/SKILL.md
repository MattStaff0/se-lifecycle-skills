---
name: se-use-cases-and-srs
description: Use when turning features into use cases, writing or reviewing use cases (preconditions, main success scenario, extensions), writing an SRS, EARS functional requirements, or quantified quality attributes, or updating docs/traceability.md.
---

# SE Use Cases, SRS, and Traceability

Work with the developer on the development contract: the use cases, the software requirements specification, and the traceability matrix. In the course: **week 4** (with business rules, which `se-requirements-foundation` owns), reviewed at **Checkpoint 1**; traceability §3 is added in **week 8**.

**Precondition:** the vision and scope has its features (`FEAT-<slug>`, §4.2). Use case areas derive from features; if there are none yet, stop and go back to `se-requirements-foundation`.

## Course templates

The course templates (`tcu-cosc-40943/course-templates`) are the authority. This skill adds the workflow around them and never restates a template's sections; where this skill and a template disagree, the template wins, and say so.

1. **Find the current copy.** Local clone: `$COURSE_TEMPLATES`, else `~/Desktop/tcu-fall-2026/software-engineering/course-templates`. Run `git -C <clone> fetch` (if fetching is not possible, say when the clone was last updated, `git -C <clone> log -1 --format=%cd`, and continue); if the clone is behind, show the developer `git -C <clone> log --oneline HEAD..@{u}` and pull only on a yes. With no clone: `gh api -H "Accept: application/vnd.github.raw" repos/tcu-cosc-40943/course-templates/contents/<path>`.
2. **Copy before writing, never over.** Before drafting, copy the templates this step needs into the target repo, on a branch, with no-clobber (`cp -n`). Report what was copied and what already existed. Never draft a course document from scratch, and never overwrite a filled one. When a template gains a section later (traceability §3 in week 8), paste that section into the existing file rather than copying the file again.
3. **Fill underneath the instructions.** Each section opens with italic `[bracketed]` instructions: follow them, run the section's checklist, write underneath, and leave the instructions in place until the document is finished. Worked examples taken from another project (Project Pulse, the Cafeteria Ordering System) come out when you write your first real entry in that section, because their identifiers would pollute the ID checks; the instructions stay.
4. **Delegate vs. keep human.** Each template ends with a "Working … with your agent" section: draft only what its Delegate list allows; for each Keep-human item, explain the choice to the developer and wait for their answer.

| Course template | Target |
|---|---|
| `requirements/use-cases.md` | `docs/requirements/use-cases.md` |
| `requirements/software-requirements-specification.md` | `docs/requirements/software-requirements-specification.md` |
| `traceability.md` | `docs/traceability.md` (in `docs/`, not `docs/requirements/`) |

## Standing rules

- **Build against use cases, not stories.** A user story is a placeholder for a conversation; an agent does not have the conversation, it guesses. It builds from use cases with testable preconditions, steps, and extensions.
- **Save your energy (Cockburn's precision levels).** The list of actors and goals first; then main success scenarios for the priority use cases; then every failure condition, step by step, before handling any; then the failure handling, which is where hidden business rules surface.
- **Never answer for the client.** An extension whose handling depends on a client policy cites an `OI-*` in `OPEN-ISSUES.md` until the client answers. A Business Rules field cites only a `BR-*` that exists in `business-rules.md`; a rule you expect but have not heard is an `OI-*`, never a plausible identifier.
- **Keep the numbers human.** Every threshold in the SRS's §9 came from the client, a measurement, or a decision the team can defend. The agent never supplies "99.9%" or "200 ms".
- **Cite, never copy.** Use cases cite `BR-*` by identifier only; the SRS links to `use-cases.md` from §5.1 rather than restating use cases.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 1: The use case list, and its first trace

Copy all three templates. Derive short area codes from the features. Fill `use-cases.md` §3 (area code, feature area, use cases) following its instruction: **this list comes before any single use case is written in detail.**

Coach the developer through **Larman's three tests** for each candidate, as questions to them, not a table in the document:

- **Boss test:** if the actor told their boss "I spent the morning doing this", would the boss say "good", or "is that all?" A no means it is a step ("log in"), not a use case.
- **EBP test:** one person, one place, one sitting, leaving the business data consistent and adding business value.
- **Size test:** roughly three to ten steps in its main success scenario.

Then fill `traceability.md` §1 (feature → business objectives → bare area codes; an enabling feature says so) and run both of its checks yourself.

**STOP.** The use case list goes to the client for review before any flow is detailed, and the developer sets the priorities. Wait.

### Step 2: Write the priority use cases

One `###` heading per use case (`### UC-<AREA>-<slug>: <a sentence>`) under a `##` heading per area, with exactly the template's §2 fields in its order: UC ID and Name (a verb and an object), Created By and Date Created, Primary and Secondary Actors, Trigger, Description, Preconditions (`PRE-n`, each testable by the system), Postconditions (`POST-n`), Main Success Scenario, Extensions (alternative flows and exceptions, numbered from the step they branch from: `4a`, `4a1`), Priority, Frequency of Use, Business Rules (identifiers only), Associated Information (including the data-field table), Related Use Cases, Assumptions, and Open Issues (mirrored into `OPEN-ISSUES.md`).

Write in the present tense and by intention ("The student provides the scores", never "clicks the blue Submit button"); no UI mechanics, no HTTP codes. No if/else in a main step: the happy path stays in the step, the failure becomes an extension. For every step, ask: what if the input is invalid, the thing is not found, the user cancels, the user is not allowed, or the external system is down?

Review protocol: the developer reviews each use case against the template's checklist **alone first**; then the agent reviews it; then compare the two reviews, so the developer does not anchor on the agent's list. Finish with the template's verification: read the main success scenario aloud to someone who has not read it, and stop at every question.

### Step 3: The SRS

Read the client meeting records first: the interview guide routes its sections 7, 9, and 10 (volumes and scale, constraints and rules, external dependencies) to the SRS. Then work section by section in the template's order, following each instruction. The SRS holds what is not a use case: constraints and the operating environment, assumptions and dependencies, the non-use-case functional requirements in EARS form (§5.2: ubiquitous, event-driven, state-driven, unwanted behavior, optional), data, external interfaces, and the §9 quality attributes, each with a number and the oracle that checks it, or "not applicable" with a reason. Sections the template says to link (glossary, vision and scope, use cases, business rules) are links only. The developer supplies every threshold.

### Step 4: Business rules to what enforces them

Fill `traceability.md` §2 (business rule, its source, what enforces it: a use case step or extension, or an FR) and run both checks yourself; a check the agent reports as passing is not evidence until someone verifies it. An unenforced rule may be a decision; if so, it is recorded in `OPEN-ISSUES.md`, not left blank.

### Step 5: Traceability §3

If your `docs/traceability.md` predates §3 (the template gained it in week 8), paste §3 from the current template into it. Once the file has §3, **a use case gets its row the day it is written**, with Build `❌ Not started` and a Release value.

## Gotchas

- **Untestable preconditions.** "PRE-1: the user wants to place an order" is the trigger, not a precondition; the system cannot test what someone wants.
- **UI design in a use case.** Dropdowns, tabs, modals, and status codes in `use-cases.md` lock the design before anyone has designed it.
- **Generic extensions.** The agent is good at proposing extensions; whether each one is a real path in this client's business is the developer's call (the template's Keep-human list).
