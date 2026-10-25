# Traceability

**Project:** _[name]_ · **Last checked:** _[YYYY-MM-DD, by whom]_

_[What this file is for: the links between the other documents, from the reason something exists to the code and tests that do it. Every other document owns its contents; this one owns only the links, so nothing is restated to find out what connects to what. It answers "did we specify everything we promised, and nothing we didn't?" before the code, and "this broke: what was it supposed to do, and who asked?" after. Each table ends with checks; run them yourself, because a check reported as passing by the thing that also wrote the table is not evidence. Instructions are in italic brackets: leave them in place until the file is stable.]_

## 1. Feature to slice area

_[Each feature from vision and scope, the objectives it serves (an objective is anchored once, here, and every slice under it inherits it), and the slice areas that realize it. Features and areas are many-to-many; a feature that lands on exactly one area is probably too narrow. An enabling feature that serves no objective on its own says "(enabling)".]_

| Feature | Objectives | Slice areas |
|---|---|---|
| _`FEAT-<slug>`_ | _`BO-<slug>`, …_ | _`EXP`, `FIN`, …_ |

**Checks.** 1. Every feature has at least one area (otherwise the vision promises something nobody specified). 2. Every area is reached by a feature (otherwise there is work nobody asked for).

## 2. Rule to what enforces it

_[Rules are cited, never restated; this table holds only the ID, its source (copied here on purpose, because it is what reviewers read), and what enforces it: a slice step or failure path, an FR, or a quality attribute.]_

| Rule | Source | Enforced by |
|---|---|---|
| _`BR-<slug>`_ | | _`UC-EXP-submit-report` step 4 and 4a_ |

**Checks.** 1. Every rule is enforced by something, or the decision not to enforce it is recorded in open issues. 2. Every `BR-*` cited anywhere exists in the rules file (this catches a plausible identifier the agent invented).

## 3. Slice to design, code, and tests

_[One row per slice (use case, data contract, model contract) and per non-slice FR that needs building. **Add the row the day the slice is written**, not the day it is built._

_**Build** and **Verify** are separate, so that built-but-untested stays visible:_
- _Build: `❌ Not started`, `📐 Designed` (its design pull request merged), `🟡 In progress`, `✅ Built`._
- _Verify: `✅ Verified` (tests or data tests or evaluation cover the main path and every failure path, and pass), `🟡 Partial` (the Tests cell names the gap), `🔎 None`, or blank while nothing is built._

_**Release** is the release plan: the release the slice ships in (use the project's own release names; a slice with no release is work nobody planned). Place slices by risk first, then by what stakeholders need first, then by dependency; change a Release cell only by pull request, saying why.]_

| Slice or FR | Build | Verify | Release | Design | Code | Tests |
|---|---|---|---|---|---|---|

**Checks.** 1. Every slice and FR has a row, and every row has a Release. 2. Nothing reads Verified without a test you can name: open each one and confirm it covers the failure paths.

## Working this document with your agent

_[Delegate: running the checks and reporting failures; listing every ID cited anywhere in the docs that has no definition; regenerating table 2 from the slices' rule fields._

_Keep human: whether an area with no feature is a missing feature or work that shouldn't exist, and whether an unenforced rule is an oversight or a decision. Both are questions about the stakeholders' business.]_
