# Glossary

**Project:** _[name]_ · **Last revised:** _[YYYY-MM-DD]_

_[What this file is for. An agent does not read your repository; it searches it with the words you give it. If the code says `PeerEvaluation` and you ask about "peer review", it finds nothing and invents a second one. This file fixes one word per concept, used the same way in conversation, code identifiers, column names, and UI text. Leave these instructions in place until the glossary is stable.]_

## Which terms earn a place

_[Skip words everyone already knows. Write these:_

- _**Terms two stakeholders use differently.** The most valuable entry there is; a report that silently mixes the two meanings is wrong in a way nobody notices._
- _**Terms that sound generic but are not:** "active", "complete", "submitted", "week", "customer". Asking what makes something active often uncovers a rule._
- _**Metric definitions** (data and ML projects): exactly what is counted, over what window, excluding what. Link the computation rule (`BR-*`) that owns the formula._
- _**Every acronym**, spelled out._
- _**Terms the team invented.** Record them, then consider using the stakeholder's word instead.]_

## Conventions

_[- One entry per concept. Two words for one thing: pick one, and list the other under **Synonyms** so nobody names a class or a table after it._
_- Alphabetical, so anyone can find a term without searching._
_- Define the concept, not the implementation: "a weekly record of the work a student did", not "a row in the `war` table"._
_- Use the stakeholder's word when they have one._
_- The term itself is the identifier; cite it by writing it, spelled the same everywhere.]_

## Terms

_[One `###` heading per term, alphabetical: a one-to-three-sentence definition, then **Synonyms** (words never to use for it), **Not to be confused with**, and **Source** (who said it, when, or the document) lines where they help. Example of the format, delete it when you add your first term:]_

### Active customer

A customer with at least one completed order in the last 90 days, counted on the day the report runs.

**Synonyms:** live customer, current customer (never use).
**Not to be confused with:** registered customer (anyone with an account).
**Source:** Head of Sales, interview 2026-01-14. Formula: `BR-active-customer-window`.

---

_**Checklist:** Does every term two stakeholders use differently have an entry? Is every acronym spelled out? Is each definition about the concept, not the code? Does every metric name its computation rule?_

## Working this document with your agent

_[Delegate: listing candidate terms from interview notes and the brief; finding places in the docs and code where a synonym is used instead of the defined term; checking alphabetical order._

_Keep human: what a term means. When two stakeholders disagree on a definition, that is a question for them (an `OI-*`), never a choice the agent makes.]_
