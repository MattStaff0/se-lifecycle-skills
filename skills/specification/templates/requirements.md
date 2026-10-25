# Requirements

**Project:** _[name]_ · **Version:** 0.1

_[What this file is for: everything the system must do or be that is not a slice contract: constraints, the environment it runs in, cross-cutting behavior, data, interfaces, and the quality attributes with numbers. The slices (use cases, data contracts, model contracts) are requirements too; this file links to them and never restates them. Instructions are in italic brackets: fill in underneath and leave them in place until the document is stable._

_**Identifiers**, all name-based slugs: `CO-` constraints, `OE-` operating environment, `AS-`/`DE-` technical assumptions and dependencies, `FR-<AREA>-<slug>` non-slice functional requirements, `DI-` data requirements, `UI-`/`SI-`/`CI-` user, software, and communications interfaces, and quality attributes `USE-` usability, `PER-` performance, `SEC-` security, `AVL-` availability, `ROB-` robustness, `SCA-` scalability, `MNT-` maintainability, `DQ-` data quality, `MQ-` model quality.]_

## 1. Links

_[Link, don't summarize: the glossary, vision and scope, the slice contract files, the business rules.]_

## 2. Constraints and environment

_[**Constraints** (`CO-*`): what is fixed by something outside the team (a required platform, a budget, a regulation, who maintains it after hand-off). A technology the team chose is a decision, recorded in the architecture, not a constraint. **Operating environment** (`OE-*`): where it runs and for whom. **Technical assumptions and dependencies** (`AS-*`, `DE-*`): each with what you would do if it fails. Keep each slug unique across this file and vision and scope.]_

## 3. Non-slice functional requirements

_[Behavior no single slice owns: background jobs, cross-cutting validation, audit, notifications, retention sweeps. If a slice step already says it, it belongs in that slice, not here. Write each in one EARS shape:_

- _**Ubiquitous:** The system shall …_
- _**Event-driven:** When <trigger>, the system shall …_
- _**State-driven:** While <state>, the system shall …_
- _**Optional feature:** Where <feature is included>, the system shall …_
- _**Unwanted behavior:** If <condition>, then the system shall …_

_Each as `**FR-<AREA>-<slug>:** <statement>`, with the slices or rules it serves. Every one needs an oracle: how a test would show it holds.]_

## 4. Data

_[The domain model as a mermaid `classDiagram` of the business concepts (not the database schema). Personal and regulated data: what, where, who may see it. Retention and disposal. For data projects, the data contracts own each output's schema; link them.]_

## 5. External interfaces

_[**User interfaces** (`UI-*`): only the requirements, not the design. **Software interfaces** (`SI-*`): every external system, what is exchanged, its owner, and what happens when it is unavailable. **Communications** (`CI-*`): email, messaging, protocols. Link an API's own documentation rather than transcribing it.]_

## 6. Quality attributes

_[Each attribute as `**<PREFIX>-<slug>:** <statement with a number>`, with **how it is checked** (the oracle: a test, a measurement, a review) and **where the number came from** (a stakeholder, a measurement, or a team decision you can defend). An adjective is not a quality attribute: "fast", "secure", and "accurate" become a number or an `OI-*`. A category that does not apply says "not applicable" with a reason. Data projects: freshness, completeness, accuracy, reproducibility as `DQ-*`. ML projects: the target metric over the baseline, failure-slice thresholds, calibration, fairness, inference latency and cost as `MQ-*`.]_

---

_**Checklist:** Is every constraint fixed by something outside the team? Is every FR in an EARS shape with an oracle, and not already a slice step? Does every external system have an interface entry with its failure behavior? Does every quality attribute have a number, an oracle, and an origin?_

## Working this document with your agent

_[Delegate: converting prose into EARS shapes; finding behavior repeated across several slices that belongs in section 3; checking that every cited ID exists in the document that owns it; drafting an oracle for an attribute stated only as an adjective._

_Keep human: the numbers. Every threshold in section 6 is a commitment someone has to live with, and an agent will supply a plausible one nobody asked for and nobody can meet.]_
