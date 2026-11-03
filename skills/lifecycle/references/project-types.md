# Project types: how each step changes

The cycle is the same for every kind of project. What changes is the shape of a **slice**, what counts as a failure path, which quality attributes dominate, and what the tests check. A project can mix types (an app with a pipeline and a model behind it); each slice declares its own.

Record the project's types in `AGENTS.md` (the `charter` skill does this), so every skill knows which shapes apply.

## The slice, by type

| | Application feature | Data pipeline / data product | ML model |
|---|---|---|---|
| **Slice ID** | `UC-<AREA>-<slug>` | `DP-<AREA>-<slug>` | `MD-<AREA>-<slug>` |
| **Contract** | Use case (`specification/templates/use-cases.md`) | Data contract (`specification/templates/data-contracts.md`) | Model contract (`specification/templates/model-contracts.md`) |
| **The goal it serves** | An actor's goal in one sitting | A consumer's question the data must answer, at a grain and freshness | A decision the model supports, and what an error costs |
| **The "steps"** | Actor and system, alternating | Source → transform → sink stages | Input → features → prediction → action |
| **The failure paths** | Extensions: invalid input, not found, cancelled, not allowed, external system down | Late or missing data, schema drift, duplicates, nulls, out-of-range values, partial loads, backfill, reruns | Out-of-distribution input, low confidence, missing features, drift, label noise, the model unavailable |
| **The rules it cites** | Business rules (`BR-*`) | Business rules, including **metric definitions** ("active user", "revenue") | Business rules, including the decision policy (thresholds, who overrides) |
| **The test list checks** | Each flow and extension (unit, integration) | Data tests per output (uniqueness, not-null, accepted values, referential integrity, freshness, row-count bounds) plus unit tests on transforms | Data validation, training reproducibility, evaluation against the baseline on the held-out set and each failure slice, and serving tests |

## Discovery, by type

- **Application:** who the users are, the process today, the pain points with numbers.
- **Data:** every source and its owner, every consumer and what they decide with the data, the metric definitions in the stakeholders' words (the glossary matters most here), freshness needs, volumes, personal and regulated data.
- **ML:** the decision and who makes it today, the baseline it must beat (a heuristic, a rule, a person), the cost of a false positive versus a false negative, the labels that exist and how good they are, latency and cost limits, and fairness or explainability constraints.

The most dangerous invented fact differs by type: an invented pain-point number (application), an invented metric definition (data), an invented target metric or baseline (ML). All of them are `OI-*` until a stakeholder or a measurement supplies them.

## Quality attributes, by type

Every project uses the general prefixes in `specification/templates/requirements.md` (`PER-` performance, `SEC-` security, `AVL-` availability, `ROB-` robustness, `SCA-` scalability, `MNT-` maintainability, `USE-` usability). Data and ML projects add:

- **`DQ-*` data quality:** freshness, completeness, accuracy, uniqueness, lineage, reproducibility of a rerun.
- **`MQ-*` model quality:** the target metric and its threshold over the baseline, calibration, robustness on named failure slices, fairness across named groups, inference latency and cost.

The human supplies every number. "99.9%", "200 ms", and "0.95 AUC" are invented unless a stakeholder, a measurement, or a defended team decision gave them.

## Architecture, by type

The architecture-of-record template is the same. What usually fills it:

- **Application:** front end, back end, database, external services. The classic first decision: one deployable or several.
- **Data:** sources, ingestion, storage layers (raw, cleaned, served), transformation, orchestration, serving (warehouse tables, APIs, dashboards), catalog and lineage. Typical key decisions: batch or streaming, warehouse or lake, the orchestration tool, full or incremental loads.
- **ML:** the feature pipeline, training pipeline, experiment tracking, model registry, serving (batch scoring or online), monitoring and retraining. Typical key decisions: batch or online inference, build or buy the model, retraining trigger.

**Resist unjustified infrastructure.** Agents default to Kafka, Spark, Kubernetes, and feature stores for data that fits on a laptop. For every component, ask which ranked quality attribute forces it.

## Design, by type

The design-of-record template has the same sections for every type; a data or ML slice also fills its type's section:

- **Data:** a lineage diagram (sources → stages → outputs, each stage labeled with the contract step it implements), the output schema (grain, keys, column meanings citing the glossary), the load strategy (full or incremental, idempotency, backfill), and the data tests per output.
- **ML:** the pipeline diagram, feature definitions (and where each is computed, to prevent training/serving skew), the split strategy (and the leakage it prevents), the evaluation plan (metric, baseline, failure slices, the threshold that gates release), and the fallback when the model is unavailable or unsure.

## Build, by type

The trunk-and-leaves cut still holds:

- **Trunk:** shared schemas, transforms other stages depend on, feature definitions, the model interface, contracts.
- **Alone:** a security rule, a schema migration or table definition, a permissions grant.
- **Leaves:** individual jobs, endpoints, dashboards, report queries, batch scoring runs.

**Experiments are not trunk.** Exploratory notebooks answer a question; their result is written down (`design/templates/experiment.md`) and the production code is rebuilt from the design. Review checks that no notebook code was merged as production.

## Review, by type

The checklist is the same; the contract it reviews against differs. For data: does every output have its data tests, and does a rerun produce the same result? For ML: is the evaluation on the held-out set only (no test-set peeking, no leakage across the split), does it beat the baseline on every named failure slice, and is the fallback tested?
