# _[Area name]_ Design

> **Realizes:** _[every slice ID this document designs (`UC-`, `DP-`, `MD-`) and any `FR-*`, appended as each is designed]_
> **Depends on:** _[the rules, constraints, and cross-cutting requirements it builds on, by ID]_
> **See:** _[links to the slice contracts, the requirements, and the architecture-of-record]_
> **Designed against:** _[the commit of the main branch the code was read at]_

_[What this file is for: how the code will realize this area's slices, written **before** the code and approved by pull request before anyone builds from it. It is the last document the agent reads before writing code, so whatever it leaves open, the agent invents. It cites slices, rules, and requirements by ID and says only how, never restating what. Once code exists, link files instead of describing them._

_**One document per area, revised in place.** The overview, component diagram, and data model are revised as the area grows; the flows, contract rows, and test rows are appended per slice. Never a per-slice copy of the whole skeleton._

_**How much to write: pin what a wrong guess would break** (a requirement, a rule, a quality attribute, or the contract between two parts of the team); leave names, field types, and exact formats to the agent. In a new area with no code yet, never drop the expected file paths, the contract, the decisions, or the test list._

_**Write it in this order:** 1. sketch the main flow and the contract rows yourself first (about twenty minutes); 2. the agent drafts this document in plan mode; 3. compare the two and decide each difference; 4. run the questions test (last section); 5. open a pull request a teammate reviews and merges. Nobody builds from this design until it is merged. Instructions are in italic brackets: leave them in place.]_

## Overview

_[One paragraph: what the area does, which components of the architecture-of-record it lives in (link them), and what it reads from or calls in other areas. If this design changes the architecture, say so and list it under "Changes to the architecture-of-record".]_

## Components

_[One diagram for the whole area (a mermaid `classDiagram` for code; a component flowchart for pipelines), and one line per class, module, job, or file: what it does and whether it is **new** or **reused**, with its owning package once code exists (`<<reused, billing>>`). For a new area, list the expected file paths, from the architecture's conventions, all marked new.]_

## Flows

_[One diagram per slice, under its own `###` heading (the anchor traceability cites): a `sequenceDiagram` for a use case, a lineage flowchart (sources → stages → outputs) for a data contract, a pipeline flowchart (data → features → training → evaluation → serving) for a model contract. **Label every interaction or stage with the contract step it implements** (`(step 4)`, `(2a)`), so a reviewer sees every step realized and nothing invented. Use `alt` blocks or branches for failure paths that do more than return an error.]_

## Contract

_[The interface other people build against, one row per item, appended per slice. Never dropped: before the code exists it is the only thing both sides read._

- _**Application:** each endpoint, job, or message: caller and who may call it (scoped how), request, success response, and the error each failure path returns, in the error shape the architecture fixes._
- _**Data:** each output: its location, grain and keys, schedule, and what consumers see when a check fails (stale flag, empty partition, nothing published)._
- _**ML:** the prediction interface: inputs, output, the threshold or how it is applied, the response when the model is unavailable or unsure.]_

| Item | Caller or consumer | Input | Success | Failure, by path |
|---|---|---|---|---|

## Key decisions

_[Only what a reader could not recover from the code: an invariant, who may do this and how it is enforced, a trade-off, a reuse choice. **Each names what was chosen, what was rejected, and why.** No rival you can name: probably not a decision. A decision that affects the whole system is a `KD-*` in the architecture instead.]_

## Data model

_[The change this area makes: new tables, entities, columns, the migration that adds them. An ER diagram when there is more than one new table. "No change" with the reason when there is none.]_

## Data and ML specifics

_[Delete the parts that do not apply._

- _**Pipelines:** the load strategy (full or incremental), how a rerun stays idempotent, how a backfill runs, and the data tests per output._
- _**Models:** each feature and where it is computed (the same code at training and serving, or the skew it risks); the split strategy and the leakage it prevents; the evaluation plan (metric, baseline, failure slices, the threshold that gates release); experiment tracking; the fallback when the model is unavailable or below threshold.]_

## Reuse and cross-cutting

_[Existing services and conventions this area uses rather than rebuilds (authentication, error handling, notifications, the clock, data-quality checks), citing the architecture's crosscutting concepts by name.]_

## Tests

_[One row per path: the main path and **every** failure path of every slice here, appended per slice, plus authorization. The level (unit, integration, data test, evaluation) and what it asserts. No code. This list is the definition of done.]_

| Slice, path | Level | Asserts |
|---|---|---|

## Changes to the architecture-of-record

_[Each change this design needs to the map (a new component, a moved responsibility, a new dependency), made in the architecture-of-record in the same pull request and listed here in one line. Or "None".]_

## Open questions and risks

_[What this design could not settle and who can; what could go wrong that you are accepting. A stakeholder question also goes in the open-issues file.]_

---

## Working this document with your agent

_[Delegate: drafting the diagrams from your sketch and the contract; listing the existing files a component should extend; filling the contract rows' inputs and outputs; proposing a test row per failure path; drafting the rejected alternative for a decision already made; checking that every cited ID exists._

_Keep human: which side of the "would a wrong guess break something" line each detail falls on; every key decision; and the approval, which is a teammate's merged review, not the drafter's own reading._

_**The failure to watch for: scope nobody asked for.** An agent drafting a design adds what designs it has read usually have: an admin override, a retry queue, a cache, a status column, a notification "while we are here". Every interaction or stage should name the contract step it implements; one that names none is a missing requirement (to the stakeholders) or scope creep (cut it).]_

## The questions test (before the pull request)

_[Start a **fresh** agent session in plan mode, so it carries nothing from the session that drafted this. Give it only the slice contract, the rules it cites, the architecture's crosscutting concepts, and this document, and ask:_

> _List every point you would still have to guess to implement `<slice ID>`. For each, say what you would guess and whether a wrong guess could violate a requirement. Do not write code._

_For each item, fix this document or keep the guess with one line saying why a wrong guess breaks no requirement. Put the list, with your answer to each, in the pull request description in a collapsed `<details><summary>Questions test</summary> … </details>` block. The design is ready when nothing on the list could break a requirement.]_
