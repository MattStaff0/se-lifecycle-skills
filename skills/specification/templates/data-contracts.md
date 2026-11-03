# Data Contracts

**Project:** _[name]_ · **Version:** 0.1

_[What this file is for: each pipeline or data product as a contract between the people who own its sources and the people who use its outputs, including every way the data can arrive wrong. Each contract is a slice: designed, built, tested, traced, and closed as one. An agent building a pipeline from "load the orders into the warehouse" guesses the grain, the keys, what happens to a late file, and what a rerun does; this contract leaves it nothing to guess. Instructions are in italic brackets: fill in underneath and leave them in place until the document is stable._

_**Identifiers:** `DP-<AREA>-<slug>` for a contract, `BR-<slug>` for rules (including metric definitions), `DQ-<slug>` for data-quality requirements, `OI-n` for open questions.]_

## 1. The fields

_[Every contract uses these fields, in this order:_

- _**DP ID and name:** what it delivers, for whom ("Daily revenue by region for finance")._
- _**Consumer and question:** who uses the output and what they decide with it. A pipeline nobody can name a consumer for is not a slice yet._
- _**Grain and keys:** what one row means ("one row per region per day") and what makes it unique._
- _**Inputs:** each source, its owner, how and when it arrives, its expected volume, and where its schema is documented._
- _**Stages:** numbered steps from source to output (`1` ingest, `2` validate, `3` clean, `4` join, `5` aggregate, `6` publish), each by intention, not tool ("deduplicate orders on order ID", not "run the dbt model")._
- _**Outputs:** each table, file, or API, with the schema below._
- _**Freshness and schedule:** when the output must be ready and how stale it may be, citing a `DQ-*` requirement for the number._
- _**Quality checks:** per output: uniqueness of the key, not-null columns, accepted values, referential integrity, row-count bounds, freshness. These become the data tests._
- _**Failure paths:** numbered from the stage they branch from (`1a`, `2a`), each with what the pipeline does. For every stage ask: the source is late or missing? the schema changed? duplicates? nulls or out-of-range values? a partial load? a rerun or a backfill (is it idempotent)? Every validation stage has at least one._
- _**Business rules:** `BR-*` IDs only, including every metric definition the outputs compute._
- _**Personal and regulated data:** which columns, how they are protected, how long they are kept and how they are disposed of._
- _**Priority, related contracts, assumptions, open issues** (open issues mirrored into the open-issues file).]_

| Output column | Type | Meaning (glossary term) | Nullable | Rule or check |
|---|---|---|---|---|

## 2. Contract list

_[Write this list before detailing any single contract, and review it with the consumers: a missing output found here costs minutes. One row per area.]_

| Area code | Area | Contracts |
|---|---|---|
| _[FIN]_ | _[Finance reporting, from `FEAT-...`]_ | _[`DP-FIN-daily-revenue`, …]_ |

## 3. Contracts

_[One `##` heading per area, one `###` per contract: `### DP-<AREA>-<slug>: <what it delivers>`.]_

---

_**Checklist for each contract:** Is there a named consumer and question? Are the grain and keys stated? Does every source name an owner? Does every stage that can fail have a failure path, including late data, schema drift, and reruns? Does every output have its quality checks? Are metric definitions cited as rules, not re-derived here? Could someone write the data tests from it without asking you?_

## Working this document with your agent

_[Delegate: drafting stages and output schemas from the source documentation; proposing failure paths and quality checks you missed; turning a finished contract into a first list of data tests; checking that every cited `BR-*` and glossary term exists._

_Keep human: every metric definition, every freshness and quality threshold, which failure paths are real for these sources, and what the pipeline should do in each (stop, quarantine, alert, proceed). Those are the consumers' and owners' decisions.]_
