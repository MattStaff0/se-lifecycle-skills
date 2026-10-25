# Use Cases

**Project:** _[name]_ · **Version:** 0.1

_[What this file is for: application behavior, written as the dialogue between an actor and the system, including every way it can fail. Each use case is a slice: the unit that is designed, built, tested, traced, and closed. An agent building from a story with gaps fills them silently; a use case with testable preconditions and every extension leaves it nothing to guess. Instructions are in italic brackets: fill in underneath and leave them in place until the document is stable._

_**Identifiers:** `UC-<AREA>-<slug>`: a short area code (3 letters) and a name-based slug, never a number, so inserting one renumbers nothing. Rules are cited as `BR-<slug>`, open questions as `OI-n`.]_

## 1. The fields

_[Every use case uses exactly these fields, in this order:_

- _**UC ID and name:** the ID and a verb-object name for the goal ("Submit an expense report"), not a noun ("Expense management", which is a feature)._
- _**Created by, date.**_
- _**Primary and secondary actors:** the primary actor starts it; secondary actors take part._
- _**Trigger:** the event or action that starts it._
- _**Description:** one or two sentences: why, and the outcome._
- _**Preconditions** (`PRE-1`, …): what must already be true, each something **the system can test** ("the user is signed in"), never a wish ("the user wants to …", which is the trigger)._
- _**Postconditions** (`POST-1`, …): what is true at success._
- _**Main success scenario:** numbered steps alternating actor and system, in the present tense, by intention, not mechanics ("the employee provides the receipt", never "clicks the blue Upload button"), ending with the goal achieved. No if/else in a step: the failure becomes an extension._
- _**Extensions:** numbered from the step they branch from (`4a`, with sub-steps `4a1`): **alternative flows** that still succeed, and **exceptions**. For every step ask: invalid input? not found? cancelled? not allowed? an external system down? Every "the system validates" step has at least one._
- _**Priority**, and **frequency of use** (how often, which tells the architecture what matters)._
- _**Business rules:** `BR-*` IDs only, never their text._
- _**Associated information:** data fields (table below), quality attributes that apply, and what a systemic failure (a timeout) leaves behind: rolled back, completed, or partial._
- _**Related use cases, assumptions, open issues** (open issues mirrored into the open-issues file).]_

| Field | Type | Validation | Who may see or set it | Glossary term |
|---|---|---|---|---|

## 2. Use case list

_[Write this list before detailing any single use case: it is the cheapest thing to review with stakeholders, and a missing area found here costs minutes. One row per area. Before a use case earns a row, check it against three tests: would the actor's manager say "good" if they spent the morning doing it (not "is that all?"); does one person finish it in one sitting, leaving the data consistent; and is it roughly three to ten steps? A step like "log in" is not a use case.]_

| Area code | Area | Use cases |
|---|---|---|
| _[EXP]_ | _[Expenses, from `FEAT-...`]_ | _[`UC-EXP-submit-report`, …]_ |

## 3. Use cases

_[One `##` heading per area, one `###` heading per use case: `### UC-<AREA>-<slug>: <the actor does the thing>`. Start with the priority ones.]_

---

_**Checklist for each use case:** Is the name a verb and an object? Can the system test every precondition? Do steps alternate actor and system? Does every step that can fail have an extension? Are rules cited by ID only? Could a tester write tests from it without asking you anything? Read the main success scenario aloud to someone who hasn't seen it, and stop at every question: each is a missing step or extension._

## Working this document with your agent

_[Delegate: drafting the main success scenario once the trigger and goal are known; proposing extensions you missed (it is good at this); turning a finished use case into a first list of tests; checking that every cited `BR-*` exists._

_Keep human: whether this is one use case or three, its priority, and whether each extension the agent proposed is a real path in this stakeholder's business or a generic one it has seen elsewhere. Review each use case yourself first, then compare with the agent's review, so you don't anchor on its list.]_
