# Architecture-of-Record

**Project:** _[name]_ · **Version:** 0.1 · **Last revised:** _[YYYY-MM-DD]_

_[What this file is for: the one map of the whole system: every slice area, component, external system, and the few decisions that are expensive to change. It is **breadth-complete and depth-shallow**: every part is named, and nothing is designed deeper than its responsibility. How one slice works inside its component is a design-of-record, written later, one per area, against real code._

_What it is not: a copy of the requirements. It cites `CO-*`, `SEC-*`, `PER-*`, `DQ-*`, `MQ-*` and the rest by ID and never restates them; a threshold that appears here and nowhere in the requirements is a requirement in the wrong file._

_What belongs here: decide now what is hard to reverse, affects the whole system, and is forced by a quality attribute or a constraint (how many deployables, where the data lives, batch or streaming, how users sign in, which external systems you depend on). Leave class names, endpoint shapes, table columns, and packages to the per-area designs._

_The sections follow **arc42** (Starke and Hruschka), with **C4** diagrams (Simon Brown) in mermaid. A section that needs running code waits for it; never fill it with guesses to look finished. Instructions are in italic brackets: leave them in place until the document is stable._

_**Identifiers:** `KD-<slug>` key decisions, `QS-<slug>` quality scenarios, `RISK-<slug>` technical risks, `TD-<slug>` technical debt.]_

## 1. Introduction and goals

### 1.1 Requirements overview

_[Link the requirements and the slice contracts; don't summarize them.]_

### 1.2 Quality goals

_[The three quality attributes that most shape the system, in priority order, with their requirement IDs and why each shapes the architecture. If you can't rank them, ask the stakeholder which they would give up first.]_

| Priority | Quality goal | Requirement IDs | Why it shapes the architecture |
|---|---|---|---|

### 1.3 Stakeholders

_[Link vision and scope's stakeholder profiles.]_

## 2. Constraints

_[List the `CO-*` and `OE-*` IDs; add a sentence only where a constraint narrows a choice in a way its text doesn't make obvious. A stack the team chose is a decision (section 9), not a constraint.]_

## 3. Context

_[A C4 context diagram: the system as one box, every kind of user, and every external system the requirements name (`SI-*`, `CI-*`, `DE-*`), with what flows between them. A box with no interface requirement behind it means the requirements are missing one.]_

## 4. Solution strategy

_[Three to five one-sentence bullets: the few moves that shape everything else (the technology base, the top-level split, how each quality goal is met, any build-or-buy choice). Each cites the `KD-*` or quality goal that explains it.]_

## 5. Building blocks

### 5.1 Containers

_[A C4 container diagram: the separately running or separately stored pieces (front end, back end, database; or ingestion, storage layers, orchestration, serving; or feature pipeline, training, registry, serving, monitoring). Name each container's technology; label every arrow with what it does and its protocol. Every external system from section 3 appears again, attached to the container that talks to it.]_

### 5.2 Areas and components

_[One row per slice area, plus one per cross-cutting component no area owns (authentication, notifications, an external integration, a shared feature store). Responsibility is one sentence; Depends on names components and external systems, never classes. Status is `provisional` until a slice has been built through it, then `proven`.]_

| Area | Component | Responsibility | Depends on | Status |
|---|---|---|---|---|

_[Check: every slice area has a row, and every external system appears in some Depends on cell.]_

## 6. Runtime view

_[After the proving slice runs: one sequence diagram for it, through the containers named as in 5.1, including what happens when an external system fails or is late. Empty until then.]_

## 7. Deployment view

_[Once there is a pipeline: where each container runs in each environment, how a merged change gets there, and what state survives a restart.]_

## 8. Crosscutting concepts

### 8.1 Security

_[Four short paragraphs: the **trust boundary** (the container that is the boundary, what sits outside it, and that it covers every path the system answers, framework endpoints included); **authentication**; **authorization** (roles, and what a user may see beyond their role, which is where breaches happen); **sensitive data** (what personal or regulated data is stored, where, and which external systems receive it). Each of the last three cites the `SEC-*` it answers. Secrets: where they live, never what they are.]_

### 8.2 Other concepts

_[One short subsection per concept: the rule in one sentence, why, and the file that shows it done right once one exists. Write them before the second component is built, because the agent reinvents any convention that is not written down; then put a one-line pointer to each in `AGENTS.md`.]_

| Concept | The question it settles |
|---|---|
| Error handling | What does a failure look like to the caller, and where is it caught? |
| Time | Whose clock decides, what time zone is stored, can a test set the time? |
| API and output conventions | What shape does every response or output take? |
| Code conventions | Which libraries and idioms, and which are banned? |
| Validation | Where is input checked, and which check counts? |
| Configuration and secrets | What differs between environments, and where does it live? |
| Logging | What is logged, at what level, and what never is? |
| Persistence and concurrency | Where does a transaction begin and end; what if two writers collide? |
| Data quality and lineage (data, ML) | Where are checks run, what happens on failure, how is lineage recorded? |
| Reproducibility (data, ML) | How is a run or a model rebuilt exactly (versions, seeds, snapshots)? |
| Testing | Which kinds of test, at which layer, with what data? |

## 9. Architecture decisions

### 9.1 Architecturally significant requirements

_[The three to six requirements where a wrong guess costs a redesign, ranked by importance to the stakeholder times difficulty to achieve, using the requirements' own IDs. At least one is a `SEC-*`.]_

| Rank | Requirement | IDs | Importance × difficulty | Drives |
|---|---|---|---|---|

### 9.2 Key decisions

_[One entry per decision, the first being `KD-deployment-shape` (how many deployables, or for data and ML, how many pipelines and serving paths). Each: driving requirements, context, decision, **rejected alternative and why it lost**, trade-off, and status (Accepted, or Superseded by `KD-<new>`, never rewritten).]_

## 10. Quality scenarios

_[Link the requirements' quality attributes. Then, once the slice runs, one scenario per top-ranked requirement: source and stimulus, environment, response, measure (citing the requirement for its number), and the test that verifies it.]_

| ID | Source and stimulus | Environment | Response | Measure | Verified by |
|---|---|---|---|---|---|

## 11. Risks and technical debt

_[Technical risks and debt only, each as a mechanism ("the vendor API's limits are unknown; at 10 calls a second the nightly sync never finishes"), with a mitigation or fix. Seed it from the technical business risks and any open issue whose answer could change the architecture.]_

| ID | Type | What could go wrong, and what it breaks | Mitigation or fix | Cites |
|---|---|---|---|---|

## 12. Glossary

_[Link the project glossary.]_

---

_**Checklist:** Does every area have a component and every external system a dependent? Does every container name its technology? Does every component trace to a ranked requirement or an area (no infrastructure nobody asked for)? Is §8.1 complete? Does every §8.2 concept have its rule? Does every decision name a rejected alternative?_

## Working this document with your agent

_[Delegate: drawing the C4 diagrams from the slice list and the interfaces; checking that every area has a component and every external system a dependent; checking that every cited ID exists; drafting the rejected alternative for a decision already made._

_Keep human: the ranking in 9.1 and every key decision. Those depend on facts about the stakeholders and the team that are not in any file, and they are what the people who inherit the system will hold you to.]_
