---
name: architecture
description: Use when making system-wide architectural decisions, drafting or revising the architecture-of-record (arc42, C4 diagrams), ranking architecturally significant requirements, defining security trust boundaries or crosscutting conventions before building a second component, recording ADRs, or when an agent proposes microservices, queues, Spark, Kafka, Kubernetes, or a feature store.
---

# Architecture-of-Record

Work with the developer on the one breadth-complete, depth-shallow map of the system, shaped by ranked quality attributes and constraints rather than the feature list. It is step 4 of the cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder); for what usually fills it in a data or ML system, read `../lifecycle/references/project-types.md`.

**Preconditions:** the slice list exists (with traceability §1), and the requirements have their constraints and quality attributes, each with a number or an `OI-*` naming who will supply it. If either is missing, stop and go back to `specification`; an architecture drawn from a feature list has nothing to rank.

## Template

1. **The project's own template wins.** If `AGENTS.md`'s Documents table names a template source for the architecture (a team's template, an ADR convention, a course's), use it: if it is a git clone, `git -C <clone> fetch` first (if that is not possible, say when it was last updated, `git -C <clone> log -1 --format=%cd`, and continue) and pull only on a yes. Where it and this skill disagree, the template wins, and say so.
2. **Otherwise use the bundled template**, `templates/architecture.md`, copied with `cp -n` to the path in the Documents table (default `docs/design/architecture.md`), on a branch.
3. **Fill underneath the instructions** and leave them in place. A section that needs running code (runtime, deployment, quality scenarios) stays empty until the code exists.
4. **Delegate vs. keep human** (the template's "Working this document with your agent"): Claude draws the diagrams, runs the coverage checks, checks cited IDs, and drafts the rejected alternative for a decision already made. **The ranking of the architecturally significant requirements and every key decision are the developer's.** Claude proposes candidates and options, explains the trade-offs, and waits.

## Standing rules

- **The reversibility test.** Decide now only what is hard to reverse, affects the whole system, and is forced by a quality attribute or constraint. Leave class names, endpoints, table columns, and packages to the per-area designs.
- **Quality attributes choose the structure.** Functionality says what components exist; ranked quality attributes and constraints choose how they are arranged.
- **Resist unjustified infrastructure.** Agents default to microservices, message queues, Spark, Kafka, Kubernetes, feature stores, and extra databases. For every container or tool proposed, ask "which ranked requirement forces this?" If none does, cut it, and record the heavier design as the rejected alternative in `KD-deployment-shape`.
- **Cite, never copy.** Link the requirements, stakeholders, and glossary; cite only IDs that exist in this project's requirements.
- **Coach, don't just produce.** Before each section, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 1: Quality goals and the ranked requirements

From the requirements' quality attributes and constraints, propose candidates where a wrong guess costs a redesign, each with its ID. **The developer ranks them** by importance to the stakeholder × difficulty (three to six, at least one `SEC-*`), and picks the top three quality goals, each with why it shapes the architecture. If they cannot rank, the stakeholder's answer to "which would you give up first?" is the ranking.

### Step 2: Context and strategy

The C4 context diagram: the system as one box, every kind of user, every external system the requirements name (a box with no interface requirement behind it means one is missing). Then three to five strategy bullets, each citing the decision or quality goal that explains it.

### Step 3: Building blocks

The container diagram (technology named, arrows labeled with what they do and their protocol), then one component row per slice area plus one per cross-cutting component, all `provisional`. Check that every area has a row and every external system appears in a Depends on cell.

### Step 4: Security and crosscutting concepts

Security's four paragraphs (trust boundary, authentication, authorization, sensitive data, the last three citing the `SEC-*` they answer; secrets by location only). Then every crosscutting concept in the template's table, one rule each, written **before the second component is built**, with a one-line pointer to each in `AGENTS.md`. For data and ML, data quality and lineage and reproducibility are not optional extras: write them too.

### Step 5: Key decisions

At least `KD-deployment-shape`. **The developer makes each decision;** Claude drafts the context and the rejected alternative for it. A decision that turns out wrong is marked "Superseded by `KD-<new>`", never rewritten.

### Step 6: Before the pull request

Run the template's checklist, confirm the code-dependent sections are untouched, and draft the pull request. Push and open it only on a yes.

## After the proving slice

At least one component becomes `proven`; the runtime view gets the slice's sequence, including an external system failing or arriving late; one quality scenario is added for a top-ranked requirement, its measure citing the requirement for the number; the risks section is seeded. **A change that adds an area, component, external system, or crosscutting concern, or re-ranks a requirement, reopens this document before any design builds on it.**

## Gotchas

- **Missing cross-cutting components.** If four areas send notifications and no notification component exists, the agent builds four senders.
- **A chosen stack is a decision, not a constraint.** It goes in the key decisions with the alternative rejected; only a stack fixed by something outside the team is a constraint.
