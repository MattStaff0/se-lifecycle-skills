# Slash commands: `/estimate`, `/implement`, `/close`

Copy each block below into a repository as `.claude/skills/<name>/SKILL.md` (or `.claude/commands/<name>.md`), committed by pull request, so the whole team runs the same workflow. With the `sdd` plugin installed you can also ask the `build` skill directly (`/sdd:build`).

**Sources:** `/implement` is adapted from Project Pulse's `.claude/commands/implement.md` (github.com/Washingtonwei/project-pulse, Apache-2.0), generalized from one Spring and Vue codebase to any project type, with guard-rail preconditions and a push-on-a-yes gate added. `/estimate` and `/close` follow the two-estimate and calibration method described in Project Pulse's `docs/methodology.md`.

---

## `.claude/skills/estimate/SKILL.md`

````markdown
---
description: "Prepare a slice's estimate: count from the contract or design, leave the hours blank for people."
argument-hint: "<slice ID> [coarse|refined]"
---

# /estimate: count, never guess

Prepare the estimate for the slice in `$ARGUMENTS`. **You count; people write the hours.** Never write an hour number, a suggested range, or a "typical" figure, even when asked: the hours are the owner's call, because only they know how fast they read a diff and how long the stakeholders take to answer.

## Preconditions

- `coarse`: the slice is written. `refined`: its design has merged. If the mode contradicts the repository, say so.

## Phases

1. **Read** the slice contract (search for its ID), its open `OI-*` items, its traceability row, and for `refined` the design.
2. **Count.** Coarse: steps or stages, failure paths, cited rules, open `OI-*` items, external systems crossed. Refined: test-list rows by level, files touched (new or reused), whether there is a trunk, a separate security or migration commit, data tests or evaluation checks, and the design's open risks (a risk settled only by measuring widens the high end).
3. **Present** the table with Low and High blank, each count beside the part it informs. Coarse has four parts (Clarify, Design, Build and review, Verify), made by the team in planning poker. Refined has three (Clarify and design, what is left; Build and review; Verify), made by the owner. If closed slices carry ratios, show the matching one and the arithmetic: both ends × ratio.
4. **Offer** to add it to the slice's issue with today's date as its Made date. Draft the edit; post only on a yes. If a range looks too small for the counts, say so: cut scope, never the estimate.
````

---

## `.claude/skills/implement/SKILL.md`

````markdown
---
description: "Implement a slice from its merged design-of-record: plan, then trunk commit, pause, leaf commits, tests, traceability, pull request draft. Run /design first."
argument-hint: "<slice ID>"
disable-model-invocation: true
---

# /implement: build a slice from its design

Take the slice in `$ARGUMENTS` from its design-of-record to working, tested code. The slice contract is the contract; its rules and requirements are the acceptance criteria; the glossary fixes the vocabulary; the design is how you build it. Paths come from `AGENTS.md`'s Documents table.

## Preconditions (refuse to start)

- **The design has merged** and covers this slice. If not, stop and tell the developer to run `/design <ID>` first. Never design inside the build.
- **A fresh session.** If this session carries the design conversation or other work, ask for `/clear`. The session then continues across the trunk pause: clearing happens between gates, never within one. Anything the documents don't answer but that conversation did is a gap in the design: ask, then fix the design in this branch and name the change under Reviewer focus.
- **The guard rails are on.** `.claude/settings.json` denies reading secrets, and `AGENTS.md` has the logging and dependency rules. If not, say so before writing code.

## Standing rules

- **The spec and the design are authoritative but not infallible: challenge them.** Ambiguity, contradiction, a stale constraint, a better way: ask or push back; never silently comply or silently invent. When in doubt, or when you see a better way, shout.
- **No dependency** until a person confirms it exists and is the one intended.
- **Log identifiers, never content**; every exception handler that does not rethrow logs the exception.
- **No notebook code as production.** Exploratory code is rebuilt from the design.
- **Propose every commit; push and open the pull request only on a yes.** Never remove a `Co-Authored-By` trailer.

If no ID is given, list the slices whose Build is `📐 Designed`, in Release order, and ask which one.

## Phase 1: Read (no edits)

1. The design: its diagrams, components, decisions, contract, and test list.
2. The slice contract (search its ID): its steps or stages and every failure path. These are what the tests cover.
3. Its rules, requirements, and glossary terms.
4. The architecture's crosscutting concepts in full. Where existing code breaks a convention, follow the convention and note the drift.
5. The traceability row (is any of it built?) and the existing code the design marks reused.

If the design disagrees with the spec or the code, flag it now and fix the design (or raise a spec defect) before building on it.

## Phase 2: Plan (no approval gate)

State a short plan and continue: the acceptance criteria (main path and every failure path), the trunk, any security or migration commit, the leaves, the tests mapped to the design's test-list rows (unit, integration, data tests, evaluation), and any deviation from the design. A true blocker stops you here.

## Phase 3: Implement

On a branch, in commits a reviewer can read, cut along the design's components:

- **Trunk first, alone:** shared code many paths depend on (shared services, schemas, transforms, feature definitions, the model interface), code that changes behavior people rely on, code that is hard to roll back. Then **stop**: name the commit to read and ask the developer to say what changed and why without asking you. Continue when they can, and when they have approved any new dependency.
- **A security rule, migration, schema change, or permissions grant, alone.**
- **Then one commit per leaf:** each flow's own endpoint, view, job, report query, or scoring run.
- **Each commit carries the tests for what it adds** and passes them, run as targeted tests.
- No trunk? One commit, no pause; a security rule or migration still goes alone.

If the build forces a change from the design, update the design to match.

## Phase 4: Test and verify

1. Check the diff against the crosscutting concepts before running anything: authorization at both the route and the data (with a test for each refusal), validation where the conventions say, the injected clock, identifier-only logs, migrations for schema changes; for data, the data tests per output and an idempotent rerun; for ML, evaluation on the held-out set against the baseline and each failure slice, and the fallback. Fix a miss or record it as a deliberate deviation.
2. Run the full suites and the gate command (both in `AGENTS.md`); the developer may run slow ones. Report the result either way. A suppression, a new baseline entry, or a weakened check added to get there is named in the pull request description.
3. Where practical, run the main path end to end.
4. Report what passed, what was skipped, and why.

## Phase 5: Trace it back

In the slice's traceability row: Build `🟡 In progress` or `✅ Built`; Verify from the tests that exist (`✅ Verified` only if the main path and every failure path are tested and pass; `🟡 Partial` naming the gap; `🔎 None`); fill the Design, Code, and Tests cells, noting which failure paths each test covers.

## Phase 6: Draft the pull request

Keep the commits separate (merge with a merge commit, not a squash, so trunk and leaves stay readable). Draft the description from the project's PR template, at most three lines per section: `Closes #<issue>`; Traces to the slice and the design; Verified and Not verified; Reviewer focus (the trunk commit, any security rule or migration first, shared code, any design change). Recommend a fresh-context `/code-review` naming the slice and test list. **Push and open it only on the developer's yes.** After it merges, `/close <slice ID>`.
````

---

## `.claude/skills/close/SKILL.md`

````markdown
---
description: "Close a merged slice: honest traceability, actual hours vs. each estimate, the retrospective, charter rules, the release plan."
argument-hint: "<slice ID>"
disable-model-invocation: true
---

# /close: close a slice before the next one starts

Close the slice in `$ARGUMENTS` after its pull request merges. Draft every edit; commit, push, post, or delete nothing without the developer's yes. The owner answers the retrospective; never answer it for them.

## Preconditions

- The implementation pull request has merged. If not, stop.

## Phases

1. **Branch.** Offer to delete the merged branch; delete only on a yes.
2. **Traceability.** Build and Verify match the tests that exist; Design, Code, and Tests cells filled; the Tests cell names the failure paths covered.
3. **Nothing floating.** Spec and architecture fixes the work surfaced have merged; anything still open is an `OI-*`.
4. **Hours.** Ask where the team logs time (or for the entries to be pasted). Sum the entries starting with this slice ID. For each estimate on the issue, count only entries dated on or after its Made date: ratio = those hours ÷ ((sum of lows + sum of highs) ÷ 2). Show the arithmetic.
5. **Retrospective (five minutes).** Ask the owner: Which part missed most? What did you not know when you estimated? What will you do differently next time? Draft their answer as this slice's biggest-miss sentence in the calibration table (ask where it lives the first time), beside its estimates, hours, and ratios.
6. **Charter.** Ask the reviewer what the agent had to be corrected on more than once; propose each as a rule in `AGENTS.md`, in its own small pull request.
7. **Release plan.** With the ratios applied, do the remaining estimates still fit the date, and is the next slice still the right one? Propose any Release changes as a pull request that says why. If a promised date will slip, the stakeholders hear it now, not on the date.
8. Tell the developer to `/clear` before the next slice.
````
