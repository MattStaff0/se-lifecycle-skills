---
name: se-architecture-of-record
description: Use when making system-wide architectural decisions, drafting or revising docs/design/architectural-design.md (arc42, C4 diagrams), ranking architecturally significant requirements, defining security trust boundaries or crosscutting conventions before building a second component, recording ADRs, or when an agent proposes microservices, queues, or extra databases.
---

# SE Architecture-of-Record

Work with the developer on `docs/design/architectural-design.md`: the one breadth-complete, depth-shallow map of the system, shaped by quality attributes and constraints rather than the feature list. In the course: **week 6**, merged by pull request on the day of **Checkpoint 1** (§1–5, §8, §9); §5.2 `proven`, §6, §10.2, and §11 at **Checkpoint 2**; §7 at **Checkpoint 3**.

**Preconditions:** the use case list exists (`docs/requirements/use-cases.md` §3, with `docs/traceability.md` §1), and the SRS has its constraints and its §9 quality attributes, each with a number or an `OI-*` naming who will supply it. If either is missing, stop and go back to `se-use-cases-and-srs`; an architecture drawn from a feature list has nothing to rank.

## Course template

The course template (`tcu-cosc-40943/course-templates`, `design/architectural-design.md`) is the authority; this skill adds the workflow around it and never restates its sections. Where the two disagree, the template wins, and say so.

1. **Find the current copy.** Local clone: `$COURSE_TEMPLATES`, else `~/Desktop/tcu-fall-2026/software-engineering/course-templates`. Run `git -C <clone> fetch` (if fetching is not possible, say when the clone was last updated, `git -C <clone> log -1 --format=%cd`, and continue); if behind, show `git -C <clone> log --oneline HEAD..@{u}` and pull only on a yes. No clone: `gh api -H "Accept: application/vnd.github.raw" repos/tcu-cosc-40943/course-templates/contents/design/architectural-design.md`.
2. **Copy before writing, never over:** to `docs/design/architectural-design.md` with `cp -n`, on a branch. Report whether it already existed.
3. **Fill underneath the instructions** and leave them in place. Every section says which checkpoint it is due at; **a section not due yet stays as it is**, never filled with guesses to look finished. The Cafeteria Ordering System example rows come out as you write your own.
4. **Delegate vs. keep human** (the template's "Working this document with your agent"). Claude may draw the C4 diagrams from the use case list and the SRS's interfaces, check that every area has a component and every external system a component that depends on it, check that every cited identifier exists, and draft the rejected alternative for a decision already made. **The ranking in §9.1 and every `KD-*` are the developer's.** Claude proposes candidate rows and options, explains the trade-offs, and waits.

Filled-in example: Project Pulse's `docs/design/architectural-design.md` (`Washingtonwei/project-pulse`). Its headings differ from the template's; follow the template.

## Standing rules

- **The reversibility test.** Decide now only what is hard to reverse, affects the whole system, and is forced by a quality attribute or constraint: how many deployables, where the data lives, how users sign in, which external systems you depend on. Leave class names, endpoint shapes, table columns, and packages to the design-of-record in week 7.
- **Quality attributes choose the structure.** Functionality says what components exist; the ranked quality attributes and constraints choose how they are arranged.
- **Resist unjustified distribution.** Agents default to microservices, queues, and extra databases. For every container or pattern proposed, ask "which row in §9.1 forces this?" If none does, cut it, and record the distributed design as the rejected alternative in `KD-deployment-shape`.
- **Cite, never copy.** §1.1, §1.3, §2, §10.1, and §12 link to their owning documents. Cite only identifiers that exist in this project's SRS; a threshold that appears here and nowhere in the specification is a requirement in the wrong document.
- **Coach, don't just produce.** Before each section, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow (Checkpoint 1)

### Step 1: Quality goals (§1.2) and the ASR table (§9.1)

From SRS §9, propose candidates: the quality attributes and constraints where a wrong guess costs a redesign, each with its specification handle. **The developer ranks them** by importance to the client × difficulty (three to six rows, **at least one `SEC-*`**) and picks the top three quality goals for §1.2, each with why it shapes the architecture. If they cannot rank, the template's advice: ask the client which one they would give up first.

### Step 2: Context (§3) and solution strategy (§4)

§3: a C4 context diagram in mermaid, the system as one box, every user role, and every external system the SRS names (`SI-*`, `CI-*`, `DE-*`); a box with no interface behind it means the SRS is missing one. §4: three to five one-sentence bullets, each citing the `KD-*` or quality goal that explains it.

### Step 3: Building blocks (§5)

§5.1: the container diagram, each container's technology named, every arrow labeled with what it does and its protocol. §5.2: one row per use case area plus one per cross-cutting component (authentication, notifications, an external integration): use case area, component, a one-sentence responsibility, depends on (components and external systems, never classes), and status, all `provisional` at Checkpoint 1. Then the template's check: every area has a row, and every external system appears in a Depends on cell. Domain-first decomposition (packages by use case area, layers inside) is the design-of-record's job; note it for week 7.

### Step 4: Security (§8.1) and crosscutting concepts (§8.2)

§8.1, the four paragraphs the template asks for: **trust boundary** (the container that is the boundary, what sits outside it, and that it covers every path the deployable answers, framework endpoints included), **authentication**, **authorization** (roles, and the rule for what a user may see beyond their role), and **sensitive data** (what is stored, where, which external systems receive it, citing SRS §7.4). The last three each cite the `SEC-*` they answer. Secrets: where they will live, never what they are. Named at Checkpoint 1, detailed at Checkpoint 2.

§8.2: one short subsection for **every** concept in the template's table (error handling, time and time zones, API conventions, code conventions, validation, configuration and secrets, logging, persistence and concurrency, auditing, testing): the rule in one sentence, why, and the file that shows it done right once one exists. Write them all now, before the agent builds a second component, because each session reinvents any convention that is not written down. Then add a one-line pointer to each in `AGENTS.md`, citing the subsection. For time, the course's recurring example: inject a clock so tests can set the time, never call the system clock directly.

### Step 5: Key decisions (§9.2)

Checkpoint 1 requires exactly one, **`KD-deployment-shape`**: driving requirements, context, decision, rejected alternative, trade-off, and status (Accepted). **The developer makes the decision;** Claude drafts the context and the rejected alternative for it. A decision that turns out wrong is marked "Superseded by `KD-<new-slug>`", never rewritten.

### Step 6: Before the pull request

Run the template's checks; confirm §6, §7, §10.2, and §11 are untouched; and remind the developer that the document merges by pull request the day of Checkpoint 1, with the TA's review to follow.

## Later checkpoints

- **Checkpoint 2:** after the proving slice runs, at least one §5.2 row becomes `proven`; §6 gets the slice's sequence through the containers, named as in §5.1, including what happens when an external system fails; §10.2 gets one quality scenario, whose measure cites the SRS attribute for its number; §11 is seeded from the technical `RI-*` items and any open issue whose answer could change the architecture.
- **A change that adds an area, component, external integration, or cross-cutting subsystem, or re-ranks §9.1,** reopens this document before any area designs against it.

## Gotchas

- **Missing cross-cutting components.** If four areas send email and §5.2 has no notification component, the agent builds four email senders.
- **A stack the team chose is a decision, not a constraint.** It goes in §9.2 with the alternative rejected; only a stack fixed by something external belongs in §2.
