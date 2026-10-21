---
name: se-slice-orchestrator
description: Use when implementing a use case, feature, or vertical slice (UC-<AREA>-<slug> / FR-<AREA>-<slug>) in an existing codebase, when designing against legacy or undocumented code you did not write (design-as-built), or when coordinating parallel agents across git worktrees on one slice.
---

# SE Slice Orchestrator (one use case, start to close)

Take one use case (`UC-<AREA>-<slug>`, or a non-use-case `FR-<AREA>-<slug>`) from the specification to a closed, calibrated use case on an existing repository, without accumulating technical or cognitive debt. This skill finds which stage the slice is in, enforces each stage's gate, and hands off to the skill that does the work. The stages are the course's Phase C (`../se-new-repo-orchestrator/references/COURSE-LIFECYCLE.md`, relative to this skill's folder).

## Preconditions

- The architecture-of-record (`docs/design/architectural-design.md`) exists with §5.2 and §8. If not, stop: `se-architecture-of-record` first.
- The use case is written with its extensions. If not: `se-use-cases-and-srs`.

## Standing rules

- **Code is evidence, never authority.** On code nobody specified, never turn what it does into requirements. Each recovered behavior is called **intended** (goes in the spec), **corrected** (spec plus a bug fix), or a **defect** (an `OI-*` or an issue) by whoever owns the intent: the client, never the team alone.
- **Challenge before designing or building.** Look for ambiguity, missing extensions, and mismatches between the spec and the code; say so when you find one.
- **One owner, end to end.** Never split one use case by layer across teammates or agents. Parallel worktrees (`claude --worktree <name>`) only for independent leaves, after the trunk merges, and no more agents than pull requests someone will review today.
- **Fresh session per stage:** design, implementation, and any agent review each start from the merged documents (`/clear` between gates, never within one).
- **Coach.** At each stage, say in one or two sentences what it is for and what the developer decides in it.
- **Git:** propose commits, pushes, and pull requests, and make them only on a yes.

## Step 1: Diagnose (read only)

Find the use case's row in `docs/traceability.md` (if it has §3), its design (`docs/design/<area>.md`), its issue, and its branches and pull requests. Report the stage:

| Stage | You are here when | Gate to leave it |
|---|---|---|
| 1. Challenge (and design-as-built audit) | No design covers the use case | Spec fixes applied or logged as `OI-*`; no step depends on an open `OI-*` |
| 2. Design-of-record | Challenge done; design not merged | Design PR merged by a teammate who did not write it |
| 3. Build-context and refined estimate | Design merged; no issue or no refined estimate | Issue cites the use case and design; questions test run on it; owner's refined estimate on the issue, dated |
| 4. Trunk and leaves | Issue ready | Trunk read and explained at the pause; every test-list row has a passing test; PR drafted |
| 5. Review and close | PR open, or merged and not closed | PR approved and merged by a reviewer; use case closed with its ratios |

**STOP.** Wait for the developer to confirm the stage and the use case.

## Step 2: Run the stage

### Stage 1: Challenge, and design-as-built

Read `AGENTS.md`, the glossary, the use case, its business rules, the architecture's §5.2 and §8, and the area's existing code. If the code predates the spec, list every behavior, validation, and missing check you observe, and have the developer classify each one (intended, corrected, defect), routing questions to the client through `OPEN-ISSUES.md`. Then list every step that contradicts the code, every validation or external call with no extension, and every conflict with §8. Propose the spec fixes; apply them only on a yes. Then `se-design-of-record`.

### Stage 2: Design-of-record

Hand off to `se-design-of-record` (or the repository's `/design`): the developer's sketch first, the agent's draft in plan mode, compare, the developer's fresh-session questions test, the PR merged by a teammate.

### Stage 3: Build-context and refined estimate

Hand off to `se-implementation-and-calibration`, Step 2: the issue from `../se-implementation-and-calibration/templates/build-context-issue.md` (cite the use case and design, name the files, add only the residue), the questions test run on the issue, and `/estimate <UC-ID> refined` to count. The owner writes the hours.

### Stage 4: Trunk and leaves

Hand off to `/implement <UC-ID>` (`se-implementation-and-calibration`, Step 3), in a fresh session: the trunk commit, **the pause** (can the developer say what changed and why without asking the agent?), a security rule or migration alone, one commit per leaf. Each commit, the trunk included, carries the tests for what it adds.

### Stage 5: Review and close

The author's fresh-context review (`/code-review`, naming the use case and test list), then the human review with the triage line and the six-item checklist (`se-implementation-and-calibration`, Step 4). After it merges, `/close-use-case <UC-ID>`: Build and Verify set from the tests that actually exist (`✅ Verified` only if the main flow and every extension pass), the hours and each estimate's ratio, the retrospective into the calibration table, a charter rule for anything corrected twice, and the release plan revised.

Commands to copy into the repository: `references/SLICE-WORKFLOW-COMMANDS.md`.

## Gotchas

- **Skipping the pause.** Trunk and leaves in one giant diff get a rubber-stamped "LGTM"; stop after the trunk.
- **Reviewing only the diff.** A diff never shows a missing extension; review against the use case and the test list.
- **Logging sensitive data.** Log the stable ID and the failure reason, never a whole entity's `toString()`, user-submitted content, or personal data.
