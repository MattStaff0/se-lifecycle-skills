# Model Contracts

**Project:** _[name]_ · **Version:** 0.1

_[What this file is for: each model as a contract for the decision it supports, how good it must be compared with what is done today, and what happens when it is wrong or unavailable. Each contract is a slice: designed, built, evaluated, traced, and closed as one. An agent asked to "build a churn model" picks its own metric, its own split, and its own threshold, and reports a number that may not mean anything; this contract fixes those before any training. Instructions are in italic brackets: fill in underneath and leave them in place until the document is stable._

_**Identifiers:** `MD-<AREA>-<slug>` for a contract, `MQ-<slug>` for model-quality requirements, `BR-<slug>` for rules (including the decision policy), `OI-n` for open questions.]_

## 1. The fields

_[Every contract uses these fields, in this order:_

- _**MD ID and name:** the prediction and its use ("Flag likely churners for the retention team")._
- _**Decision and who acts:** the decision the model supports, who makes it, and what they do with the output._
- _**Baseline:** how the decision is made today (a rule, a person, nothing) and how well, with a number from a measurement or an `OI-*` to measure it. A model is only worth building if it can beat this._
- _**Cost of errors:** what a false positive and a false negative each cost, in the stakeholder's terms. This picks the metric and the threshold._
- _**Inputs and features:** each input, its source, and whether it is available **at prediction time** (a feature only known afterward is leakage)._
- _**Output:** what it returns (score, class, ranking), the operating threshold or how it will be chosen, and how the output is used._
- _**Training data and labels:** source, period, how labels are produced, and how trustworthy they are._
- _**Evaluation:** the metric, the threshold it must clear over the baseline (citing an `MQ-*` requirement for the number), the held-out set and split strategy (by time, by entity, …, and the leakage it prevents), and **named failure slices** with their own thresholds (new customers, a small region, a protected group)._
- _**Operating limits:** latency, cost per prediction, explainability, fairness constraints._
- _**Failure paths:** numbered from the step they branch from: a feature missing at prediction time, out-of-distribution input, low confidence, the model unavailable, drift detected. Each with what happens (a fallback rule, a human review, a retraining trigger)._
- _**Monitoring:** what is watched in production (input drift, prediction drift, the metric once labels arrive) and the threshold that raises an alert._
- _**Business rules:** `BR-*` IDs only, including the decision policy (thresholds set by the business, who may override)._
- _**Priority, related contracts, assumptions, open issues** (open issues mirrored into the open-issues file).]_

## 2. Contract list

_[Write this list before detailing any single contract, and review it with whoever owns the decisions. One row per area.]_

| Area code | Area | Contracts |
|---|---|---|
| _[RET]_ | _[Retention, from `FEAT-...`]_ | _[`MD-RET-churn-risk`, …]_ |

## 3. Contracts

_[One `##` heading per area, one `###` per contract: `### MD-<AREA>-<slug>: <what it predicts, for whom>`.]_

---

_**Checklist for each contract:** Is the decision and its owner named? Is there a measured baseline (or an `OI-*` to measure it)? Are the costs of each error stated? Is every feature available at prediction time? Is the split strategy stated, with the leakage it prevents? Are failure slices named with thresholds? Does every failure path say what happens? Could someone write the evaluation and the serving tests from it without asking you?_

## Working this document with your agent

_[Delegate: proposing candidate features and checking each for leakage; proposing failure slices and failure paths; drafting the evaluation plan's structure; turning a finished contract into a first list of tests and evaluation checks._

_Keep human: the baseline, the cost of each error, the target metric and every threshold, the failure slices that matter, and what happens when the model is wrong. An agent will report "0.92 AUC" against a split that leaks the future, and call it done.]_
