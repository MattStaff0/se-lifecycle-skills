# Business Rules

**Project:** _[name]_ · **Version:** 0.1

_[What a business rule is: a policy, regulation, standard, permission, or formula that belongs to the stakeholder's organization. It was true before the project and would stay true if the project were cancelled. "Only a regional manager may approve a refund over $500" is a rule._

_What a rule is not: a software requirement. What the software does to **enforce** a rule is a requirement, written in the requirements or a slice contract and citing the rule. The difference answers "who decided this, and can we change it?": a rule belongs to the organization; a requirement to the team._

_For data and ML projects, **metric definitions and formulas are rules**: how revenue is recognized, what counts as churn, how a score is computed. Reproduce them exactly; ask for the spreadsheet._

_Instructions are in italic brackets: fill in underneath and leave them in place until the document is stable.]_

## Hearing a rule

_[Rules surface in the middle of stories about something else: "only … may …", "if … then …", "it has to be calculated as …", "… unless it's been more than a year". Write the exact words down when you hear them; the rule is someone else's sentence, not yours._

_Five shapes help you recognize one, but are not a field in this file: a **fact** (always true: every order belongs to one customer), a **constraint** (who may do what), an **action enabler** (when X, do Y), an **inference** (X implies Y), and a **computation** (a formula).]_

## Identifiers

_[Each rule is `BR-<slug>`, a name coined from its gist (`BR-refund-approval-limit`), never renumbered or repointed. A rule may cite another rule. Slice contracts cite rules by ID only, never by copying their text, so each rule has one home.]_

## Every rule needs a source

_[A named document, a regulation, a page of a handbook, or the person who said it and the date. A rule nobody outside the team can be named as the source of is usually a team decision dressed as a rule: flag it as a candidate for the requirements instead. Test: if you asked the stakeholder to change it tomorrow, who would approve? If the answer is "you", it was never a rule. Where a rule is expected to change, say when.]_

## Rules

_[Group rules under topic headings that fit the project. Each rule: a bold identifier, the rule in one sentence about the business (in the stakeholder's words), then its source. Example of the format, delete it when you add your first rule:]_

### _[Topic]_

- **`BR-refund-approval-limit`:** A refund over $500 needs a regional manager's approval before it is paid.
  **Source:** Finance policy FP-12, §3 (shared by the controller, 2026-01-20).
- **`BR-active-customer-window`:** A customer is active if they completed at least one order in the 90 days before the day the report runs.
  **Source:** Head of Sales, interview 2026-01-14.

---

_**Checklist:** Does every rule have a source outside the team? Is each one sentence about the business, not about the software? Could the stakeholder change it without asking you? Does some slice or requirement cite it, and if none does, is that a decision recorded in open issues?_

## Working this document with your agent

_[Delegate: turning interview notes into candidate rules; spotting sentences in a transcript that have the shape of a rule; finding slices that a rule ought to govern but that do not cite it._

_Keep human: whether something is a rule at all, and every threshold and formula. An agent will produce a plausible-sounding policy ("late returns incur a 10% fee") that nobody ever stated.]_
