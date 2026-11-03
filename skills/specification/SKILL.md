---
name: specification
description: Use when turning features into a list of slices (use cases, data pipelines, or ML models), writing or reviewing a use case, data contract, or model contract, writing requirements, EARS statements, or quantified quality attributes (including data-quality and model-quality targets), or updating docs/traceability.md.
---

# Specification

Work with the developer on the contract the code is built from: the slice list, each slice's contract (a use case, a data contract, or a model contract, by project type), the requirements, and the traceability matrix. It is step 3 of the cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder); `../lifecycle/references/project-types.md` says which contract shape each kind of slice uses and what its failure paths look like.

**Precondition:** vision and scope has its features (`FEAT-*`). Slice areas derive from features; if there are none yet, stop and go back to `discovery`.

## Templates

1. **The project's own template wins.** If `AGENTS.md`'s Documents table names a template source for a document (a team's template folder, or another repository such as a course's), use it: if it is a git clone, `git -C <clone> fetch` first (if that is not possible, say when it was last updated, `git -C <clone> log -1 --format=%cd`, and continue) and pull only on a yes. Where that template and this skill disagree, the template wins, and say so.
2. **Otherwise use the bundled template** in this skill's `templates/` folder.
3. **Copy before writing, never over.** Copy to the path in the Documents table (defaults below) with `cp -n`, on a branch, and say what was copied and what already existed. When a traceability file exists without a section a newer template has, paste that section in rather than copying the file again.
4. **Fill underneath the instructions.** Follow each section's italic instruction and run its checklist; leave the instructions in place until the document is stable. Example entries come out when you write the first real one, because their IDs would pollute the ID checks.
5. **Delegate vs. keep human.** Draft only what the template's "Working this document with your agent" Delegate list allows; for each Keep-human item, explain the choice to the developer and wait.

| Template | Default path | When |
|---|---|---|
| `templates/use-cases.md` | `docs/requirements/use-cases.md` | application features |
| `templates/data-contracts.md` | `docs/requirements/data-contracts.md` | pipelines and data products |
| `templates/model-contracts.md` | `docs/requirements/model-contracts.md` | ML models |
| `templates/requirements.md` | `docs/requirements/requirements.md` | always |
| `templates/traceability.md` | `docs/traceability.md` | always |

## Standing rules

- **Build against contracts, not stories.** A story is a placeholder for a conversation; the agent doesn't have the conversation, it guesses. Build only from contracts with testable conditions, steps or stages, and every failure path.
- **Save your energy.** The list first; then the main path of the priority slices; then every failure condition, step by step, before handling any; then the handling, which is where hidden rules surface.
- **Never answer for a stakeholder.** A failure path whose handling depends on a stakeholder's policy cites an `OI-*` until they answer. A rules field cites only `BR-*` IDs that exist in the rules file; a rule you expect but haven't heard is an `OI-*`, never a plausible identifier.
- **Keep the numbers human.** Every threshold (latency, freshness, a model's target metric) came from a stakeholder, a measurement, or a decision the team can defend.
- **Cite, never copy.** Contracts cite rules by ID; the requirements link to the contracts rather than restating them.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 1: The slice list, and its first trace

Copy the contract templates for the project's types, plus requirements and traceability. Derive short area codes from the features and fill each contract file's list section. Coach the developer through whether each candidate is really a slice (as questions to them, not a table in the document): for a use case, would the actor's manager call a morning spent on it worthwhile, is it one person in one sitting, and is it three to ten steps; for a data contract, is there a named consumer and question; for a model contract, a named decision and baseline. Then fill traceability §1 and run its checks yourself.

**STOP.** The slice list goes to the stakeholders for review before any contract is detailed, and the developer sets the priorities. Wait.

### Step 2: Write the priority contracts

Use exactly the fields of the chosen template, in its order, one `###` heading per slice under a `##` per area. Walk every step or stage for its failure paths:

- **Use case:** invalid input, not found, cancelled, not allowed, an external system down; intention, not UI mechanics; no if/else inside a step.
- **Data contract:** late or missing source, schema drift, duplicates, nulls and out-of-range values, partial loads, reruns and backfills.
- **Model contract:** a feature missing at prediction time, out-of-distribution input, low confidence, the model unavailable, drift; and a baseline, error costs, a split strategy, and named failure slices before any metric is quoted.

Review protocol: the developer reviews each contract against its checklist **alone first**; then the agent reviews it; then compare the two. Finish with the template's last check (for a use case, read it aloud to someone who hasn't seen it).

### Step 3: The requirements

Read the interview records first (their scale, constraints, data, and external-systems sections feed this file). Then section by section: constraints and environment, non-slice FRs in EARS shapes with oracles, data, external interfaces with their failure behavior, and the quality attributes each with a number, an oracle, and an origin, or "not applicable" with a reason. The developer supplies every number.

### Step 4: Rules to what enforces them

Fill traceability §2 (rule, source, what enforces it) and run both checks yourself. An unenforced rule may be a decision; if so, it is recorded in open issues, not left blank.

### Step 5: Slice rows from the day each slice is written

In traceability §3, a slice gets its row the day it is written: Build `❌ Not started`, Verify blank, and a Release value once there is a release plan.

## Gotchas

- **Untestable preconditions.** "The user wants to submit a report" is the trigger, not a precondition.
- **Design in the contract.** Dropdowns and status codes in a use case, or tool names ("the dbt model", "XGBoost") in a contract, lock the design before anyone has designed it.
- **A metric quoted without a baseline and a split.** For a model contract, a number with no baseline, no held-out set, and no failure slices means nothing; it is an `OI-*` until all three exist.
- **Generic failure paths.** The agent is good at proposing them; whether each is real for these stakeholders and sources is the developer's call.
