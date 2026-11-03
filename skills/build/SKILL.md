---
name: build
description: Use when estimating human hours for a slice (use case, pipeline, or model), writing a build-context issue, implementing an approved design (/implement), reviewing an agent's pull request against its contract and design, or closing a slice and comparing actual hours to the estimate.
---

# Build, Review, and Calibrate

Work with the developer from an approved design to a merged, closed slice: estimating it in human hours (twice), assembling its build-context issue, building it in trunk-and-leaf commits with one pause, reviewing it against the contract, and closing it with a calibration ratio. It is the rest of the slice cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder); `../lifecycle/references/project-types.md` says what the trunk, the tests, and the review look like for data and ML.

## Standing rules

- **Estimate the people, never the agent.** The agent's session takes minutes and is no line of the estimate. Every hour is a person's: settling the slice with stakeholders, designing, reading what the agent wrote, seeing it work. **Claude counts; people write the hours.** Never propose an hour number, a range, or a "typical" figure. If the date can't hold, cut scope, never the estimate.
- **Cite, never copy.** A build-context issue cites the slice contract and its design and stops there.
- **Trunk first, pause once.** Cut commits along the design; pause after the trunk commit so the developer can say what changed and why without asking the agent.
- **Review against the contract, not the diff.** A diff shows what was added, never what was left out.
- **The traceability row stays honest.** Build (`❌ Not started`, `📐 Designed`, `🟡 In progress`, `✅ Built`) and Verify (`✅ Verified`, `🟡 Partial`, `🔎 None`, or blank) are separate; built is not verified.
- **What the agent may read and run** (secrets + untrusted input + a way to send data out is the dangerous combination): secrets denied by rule; permission checks never switched off; outside content read by a person before the agent reads it; every new dependency confirmed by a person.
- **No notebook code as production**, and no production data, credentials, or personal data in prompts, fixtures, or committed notebook output.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose each commit, the push, and the pull request, and make them only on a yes. Never remove a `Co-Authored-By` trailer. Merging is a person's decision after review.

## Workflow

### Step 1: The coarse estimate (when the slice is written)

The team makes it by **planning poker**: for each part, each person writes a range of hours without showing it, everyone reveals at once, the highest and lowest explain, and the team agrees on a range. A wide spread is a question for the stakeholders, not a number to average. Solo, the owner makes it the same way, alone. Claude prepares the counts (steps or stages, failure paths, rules, open `OI-*` items, external systems) with the hours blank.

| Part | What the hours buy |
|---|---|
| Clarify | Settling the slice with stakeholders |
| Design | The challenge, the design-of-record, until it merges |
| Build and review | Driving the agent, reading every line against the design, sending it back |
| Verify | Making each test-list row pass (tests, data tests, evaluation), running it end to end, fixing what that finds |

### Step 2: The build-context issue and the refined estimate (after the design merges)

Draft the issue from `templates/build-context-issue.md` (or the team's tracker template): the slice and design by ID and path, the files it will touch, and only the residue that lives nowhere else yet. Then run the **questions test on the issue**: a fresh session reads only the issue and what it cites and lists what it would have to guess; each gap is fixed in the spec, the charter, or the issue.

The refined estimate has three parts (clarify and design, what is left; build and review; verify), sized from the design's test list: rows, files touched, open risks. The owner sets the hours, by analogy to the closest finished slice at first, then by the team's own rate (logged hours ÷ test rows) once two or three slices have closed. It goes on the issue under the coarse one, dated. (`/estimate` in `templates/commands.md` does the counting.)

### Step 3: Implement (`/implement`)

Use the `/implement` command in `templates/commands.md`. Preconditions: the design has merged; a **fresh session** (it then continues across the trunk pause); the deny rules and the charter's logging and dependency rules are in place.

| Commit | What goes in it |
|---|---|
| **Trunk**, one commit | Shared code many paths depend on (services, schemas, transforms, feature definitions, the model interface), code that changes behavior people rely on, code hard to roll back |
| **Alone** | One security rule, migration, schema change, or permissions grant |
| **Each leaf**, one commit | New code reached only through this slice: an endpoint, a view, a job, a report query, a scoring run |

Each commit carries the tests for what it adds. **Pause after the trunk:** ask the developer to say what changed and why without asking the agent; if they can't, they read until they can. Fan leaves out to parallel worktrees only after the trunk merges, and no more agents than pull requests someone will review today. If the build finds a better way than the design, shout, and update the design in the same pull request.

### Step 4: Review against the contract

Use `templates/review-checklist.md`: the triage line, six checks, the data and ML additions, labeled comments. The author first runs a fresh-context review (`/code-review`, naming the slice and the test list) and records how each finding was handled. The human reviewer writes the triage line, works the checklist, and approves when the change leaves the code better than it was, never with a blocking comment open. The PR description follows the project's template (or `templates/pull-request.md`).

### Step 5: Close (`/close`)

After the pull request merges, before the next slice (`/close` in `templates/commands.md`): the traceability row honest, with Design, Code, and Tests filled; open findings logged as `OI-*`; actual hours summed from time-log entries that start with the slice ID; a **calibration ratio per estimate** = hours logged since that estimate was made ÷ the midpoint of its summed range; the owner's five-minute retrospective (which part missed most? what didn't you know? what changes next time?), its answer written as the slice's biggest-miss sentence in the calibration table that day; a charter rule for anything the agent got wrong twice; the release plan revised. Then `/clear`.

At a ratio of 1.5, multiply both ends of the next estimate by 1.5. The refined ratio corrects build estimates; the coarse ratio corrects the release forecast. When the ratio says a date will slip, the stakeholders hear it now, not on the date. Record and interpret the ratio honestly; it is a tool for judgment, never a score.

## Gotchas

- **Silent design drift.** A working pull request that diverges from its design turns the design into a lie: name the deviation and update the design in the same pull request.
- **Logging a whole object.** Its default string form prints every field, personal data and tokens included. Log identifiers and the failure reason.
- **"Add logging" or "add tests".** Asked that way, the agent logs everything and tests only what it built. Point it at the logging rule and the design's test list.
- **An evaluation number with no baseline.** For a model, "0.91 accuracy" means nothing until it is compared with the baseline on the held-out set and every failure slice.
