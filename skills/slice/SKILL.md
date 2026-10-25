---
name: slice
description: Use when building one feature, pipeline, or model (a slice such as UC-<AREA>-<slug>, DP-<AREA>-<slug>, or MD-<AREA>-<slug>) in an existing codebase, when designing against code you did not write (design-as-built), or when coordinating parallel agents across git worktrees on one slice.
---

# Slice (one slice, start to close)

Take one slice (a use case, data contract, model contract, or non-slice `FR-*`) from its contract to a closed, calibrated slice on an existing repository, without piling up technical or cognitive debt. Find which stage it is in, enforce that stage's gate, and hand off to the skill that does the work. The stages are the slice cycle in `../lifecycle/references/cycle.md` (relative to this skill's folder).

## Preconditions

- The architecture-of-record exists with its components and crosscutting concepts. If not, stop: `architecture` first.
- The slice is written with all its failure paths. If not: `specification`.

## Standing rules

- **Code and notebooks are evidence, never authority.** On code nobody specified, never turn what it does into requirements. Each recovered behavior is called **intended** (goes in the spec), **corrected** (spec plus a fix), or a **defect** (an `OI-*` or an issue) by whoever owns the intent: a stakeholder, never the team alone.
- **Challenge before designing or building.** Look for ambiguity, missing failure paths, and mismatches between the spec and the code; say so when you find one.
- **One owner, end to end.** Never split one slice by layer across people or agents. Parallel worktrees (`claude --worktree <name>`) only for independent leaves after the trunk merges, and no more agents than pull requests someone will review today.
- **Fresh session per stage:** design, implementation, and review each start from the merged documents (`/clear` between gates, never within one).
- **Coach.** At each stage, say in one or two sentences what it is for and what the developer decides in it.
- **Git:** propose commits, pushes, and pull requests, and make them only on a yes.

## Step 1: Diagnose (read only)

Find the slice's traceability row (if there is one), its design, its issue, and its branches and pull requests. Report the stage:

| Stage | You are here when | Gate to leave it |
|---|---|---|
| 1. Challenge (and design-as-built audit) | No design covers the slice | Spec fixes applied or logged as `OI-*`; no step depends on an open `OI-*` |
| 2. Design | Challenge done; design not merged | Design PR merged by someone who didn't write it |
| 3. Build-context and refined estimate | Design merged; no issue or refined estimate | Issue cites the slice and design; questions test run on it; refined estimate on the issue, dated |
| 4. Trunk and leaves | Issue ready | Trunk explained at the pause; every test-list row has a passing test; PR drafted |
| 5. Review and close | PR open, or merged but not closed | Approved and merged by a reviewer; slice closed with its ratios |

**STOP.** Wait for the developer to confirm the stage and the slice.

## Step 2: Run the stage

### Stage 1: Challenge, and design-as-built

Read `AGENTS.md`, the glossary, the slice contract, its rules, the architecture's components and crosscutting concepts, and the existing code. If the code predates the spec, list every behavior, validation, and missing check you observe, and have the developer classify each (intended, corrected, defect), routing questions to stakeholders through open issues. Then list every step that contradicts the code, every validation or external call with no failure path, and every conflict with the conventions. Propose the spec fixes; apply them only on a yes. Then `design`.

### Stage 2: Design

Hand off to `design` (or the repository's `/design`): the developer's sketch first, the agent's draft in plan mode, compare, the developer's fresh-session questions test, the PR merged by a teammate. A question only an experiment can answer goes through `design`'s experiment step first.

### Stage 3: Build-context and refined estimate

Hand off to `build`, Step 2: the issue from `../build/templates/build-context-issue.md` (or the team's tracker), cite the slice and design, name the files, add only the residue; run the questions test on the issue; `/estimate <ID> refined` to count. The owner writes the hours.

### Stage 4: Trunk and leaves

Hand off to `/implement <ID>` (`build`, Step 3), in a fresh session: the trunk commit, **the pause** (can the developer say what changed and why without asking the agent?), a security rule or migration alone, one commit per leaf. Each commit, the trunk included, carries the tests for what it adds.

### Stage 5: Review and close

The author's fresh-context review (`/code-review`, naming the slice and test list), then the human review with the triage line and checklist (`build`, Step 4). After the merge, `/close <ID>`: Build and Verify from the tests that actually exist, the hours and each estimate's ratio, the retrospective into the calibration table, a charter rule for anything corrected twice, the release plan revised.

Commands to copy into a repository: `references/commands.md`.

## Gotchas

- **Skipping the pause.** Trunk and leaves in one giant diff get a rubber-stamped "LGTM"; stop after the trunk.
- **Reviewing only the diff.** A diff never shows a missing failure path; review against the contract and the test list.
- **Logging sensitive data.** Log the stable ID and the failure reason, never a whole object, user content, or personal data.
