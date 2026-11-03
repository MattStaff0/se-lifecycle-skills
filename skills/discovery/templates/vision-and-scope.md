# Vision and Scope

**Project:** _[name]_ · **Version:** 0.1 · **Last revised:** _[YYYY-MM-DD]_

_[What this file is for: why the project exists, how anyone will know it worked, who it is for, and where the line falls between in scope and out. Every fact here comes from the brief, an interview, or a measurement; anything not yet known is an `OI-*` in the open-issues file, never a plausible guess. Instructions are in italic brackets: fill in underneath and leave them in place until the document is stable._

_**Identifiers.** Name-based slugs, never numbers, so inserting one renumbers nothing: `BO-<slug>` objectives, `SM-<slug>` success metrics, `FEAT-<slug>` features, `RI-<slug>` risks, `AS-<slug>` assumptions, `DE-<slug>` dependencies.]_

## 1. Background

### 1.1 The problem or opportunity

_[Two to four sentences in the stakeholder's terms: what is wrong or missing today, for whom, and why now.]_

### 1.2 How it works today

_[The current process as it actually happens. A mermaid flowchart with one `subgraph` per actor (person or system), then a short paragraph, the tools in use with their limits, and the pain points, each with a number from an interview or a measurement ("about 6 hours every Monday reconciling two exports"). A pain point with no number is an `OI-*` to measure.]_

### 1.3 References

_[The brief, interview records, documents the stakeholder shared.]_

## 2. Objectives and success

### 2.1 Business objectives

_[Each objective as `**BO-<slug>:** <one sentence>` with a number and a date, and the success metrics that measure it. "Make reporting faster" is a wish; "cut the weekly reconciliation from 6 hours to under 1 by June" is an objective.]_

### 2.2 Success metrics

_[How you will know each objective was achieved, not just built.]_

| ID | Indicator | Source of the number | Baseline today | Target and by when | Measures |
|---|---|---|---|---|---|
| _`SM-<slug>`_ | | | _[or `OI-*` to measure it]_ | | _`BO-<slug>`_ |

### 2.3 Vision statement

_[Fill the six lines, then read it aloud to the stakeholder: **For** (target users) **who** (need) **the** (product name) **is a** (kind of product) **that** (key benefit). **Unlike** (the current way or main alternative), **our product** (main difference).]_

### 2.4 Risks

_[Business risks only (technical ones go in the architecture). Each stated as a mechanism, not a category: "the vendor export is nightly, so a same-day dashboard is impossible", not "integration issues". Likelihood and impact are the team's estimates; label them as such.]_

| ID | What could happen, and what it breaks | Likelihood (team estimate) | Impact (team estimate) | Mitigation, or accepted and why |
|---|---|---|---|---|

### 2.5 Assumptions and dependencies

_[What must stay true for the plan to hold (`AS-<slug>`), and what the project relies on that the team does not control (`DE-<slug>`). Business-level ones here; technical ones in the requirements document.]_

## 3. Stakeholders

### 3.1 Profiles

| Stakeholder | Role | What they want from this | Attitude (supportive, neutral, skeptical) | What they decide |
|---|---|---|---|---|

### 3.2 Alternatives

_[Every option the stakeholder has, **always including the status quo**: keep doing it the current way. For each, why it falls short. If the status quo is good enough, say so.]_

## 4. Scope

### 4.1 Context

_[A mermaid diagram: the product as one box, the people and systems around it, and what flows between them.]_

### 4.2 Features

_[Each feature as `### FEAT-<slug>: <name>`, one to three sentences of capability in stakeholder terms. A feature is coarser than any one slice and spans several areas. "Create, edit, and delete X" is a list of use cases, not a feature; pull it up to the capability it serves.]_

### 4.3 MVP

_[Agreed with the stakeholder, not decided by the team. Ask the forcing question: "if we can deliver only one of these first, which one?"]_

- **In the MVP:** _[`FEAT-*` IDs]_
- **Explicitly out, and why:** _[`FEAT-*` IDs, each with its reason]_

### 4.4 Operation

_[Who runs, monitors, and maintains it after hand-off, with what skills and budget. This constrains the architecture more than any feature does.]_

---

_**Checklist:** Does every number cite a source or an `OI-*`? Does every objective have a metric, and every metric trace to an objective? Does the alternatives list include the status quo? Is every feature a capability, not a CRUD list? Did the stakeholder agree to the MVP line?_

## Working this document with your agent

_[Delegate: drafting each section from interview records and the brief; listing every question it could not answer from them (each becomes an `OI-*`); drawing the flow and context diagrams; checking that every metric traces to an objective._

_Keep human: every number, the MVP line, the vision statement's wording, and the ranking of risks. An agent will happily write "reduce processing time by 50%" when nobody said so.]_
