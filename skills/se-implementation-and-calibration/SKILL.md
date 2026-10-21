---
name: se-implementation-and-calibration
description: Use when estimating human hours for a use case, writing a GitHub build-context issue, implementing an approved design (/implement), reviewing an agent's pull request against its use case and design, or closing a use case and comparing actual hours to the estimate.
---

# SE Implementation, Review, and Calibration

Work with the developer from an approved design to a merged, closed use case: estimating it in human hours (twice), assembling its build-context issue, building it in trunk-and-leaf commits with one pause, reviewing it against the contract, and closing it with a calibration ratio. In the course: **week 8** onward (build-contexts with the questions test by the end of week 8); the proving slice runs from tag `v0.1.0` at **Checkpoint 2**, with the calibration table, coarse estimates on every undesigned use case, and a Release value on every traceability row; at **Checkpoint 3**, a ratio per person and team plus one forecast line.

## Standing rules

- **Estimate the people, never the agent.** The agent's session takes minutes and is no line of the estimate. Every hour is a person's: settling the use case with the client, designing with the agent, reading what it wrote, seeing it work. **Claude counts; people write the hours.** Never propose an hour number, a range, or a "typical" figure. If the client needs it sooner than the range allows, cut scope, never the estimate.
- **Cite, never copy.** A build-context issue cites the use case and its design and stops there; the use case already cites its business rules and quality attributes. A pasted use case is a second use case that will drift.
- **Commit trunk first, pause once.** Cut commits along the design's class diagram; pause after the trunk commit so the developer can say what changed and why without asking the agent.
- **Review against the contract, not the diff.** A diff shows what was added, never what was left out.
- **The traceability row stays honest.** Build and Verify are separate states (course template §3): Build `❌ Not started`, `📐 Designed`, `🟡 In progress`, `✅ Built`; Verify `✅ Verified` (tests cover the main flow and every extension, and pass), `🟡 Partial` (the Tests cell names the gap), `🔎 None`, or blank while nothing is built.
- **What the agent may read and run** (the lethal trifecta: secrets + untrusted input + a way to send data out). Secrets denied by rule in `.claude/settings.json`; never switch permission checks off; read outside content yourself before the agent does; a person confirms every new dependency exists and is the intended one.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose each commit, the push, and the pull request, and make them only on a yes. Never remove a `Co-Authored-By` trailer. Merging is a human's decision after review.

## Workflow

### Step 1: The coarse estimate (team, when the use case is written)

The team makes it by **planning poker**: for each of the four parts, each member writes a range of hours without showing it, everyone reveals at once, the highest and lowest explain, and the team agrees on a range. A wide spread is a question for the client, not a number to average. Claude prepares the inputs (main-scenario steps, extensions, cited rules, open `OI-*` items, external interfaces crossed) with the hours blank, and drafts its entry on the use case's issue. The four parts and their weekly-report categories:

| Part | What the hours buy | Weekly activity report categories |
|---|---|---|
| Clarify | Settling the use case with the client | `REQUIREMENTS`, `COMMUNICATION` |
| Design | The challenge loop and the design-of-record until it merges | `DESIGN` |
| Build and review | Driving the agent, reading every line against the design, sending it back | `DEVELOPMENT`, `REVIEW` |
| Verify | Making each test-list row pass, running the flow end to end, fixing what that finds | `TESTING`, `BUGFIX` |

### Step 2: The build-context issue and the refined estimate (owner, after the design merges)

Draft the issue from `templates/build-context-issue.md`: the use case and design by ID and path, the files it will touch, and only the residue that lives nowhere else yet (a decision from this week's client meeting, why this use case comes next). Then run the **questions test one level down**: a fresh session reads only the issue and what it cites and lists what it would have to guess; each gap is fixed in the spec, the charter, or the issue.

The refined estimate has three parts: **clarify and design, what is left** (the holes the build will expose), **build and review**, and **verify**. It is sized from the design's test list: count the rows, the files touched (new or reused), and the open risks. The owner sets the hours: by analogy to the closest finished change at first; after two or three closed use cases, by the team's own rate (logged hours ÷ test rows). A risk that can only be settled by measuring widens the high end. The owner writes it on the issue under the coarse one, with the date it was made. (`/estimate` in `templates/claude-commands.md` does the counting.)

### Step 3: Implement (`/implement`)

Use the `/implement` command in `templates/claude-commands.md`. Preconditions: the design is merged; a **fresh session** (`/clear`; the session then continues across the trunk pause); `.claude/settings.json` denies secrets; `AGENTS.md` carries the logging rule and the dependency rule. Commit kinds:

| Commit | What goes in it |
|---|---|
| **Trunk**, one commit | Code many paths depend on, code that changes behavior existing users rely on, code that is hard to roll back |
| **A security rule or a schema migration**, alone | One rule, one migration |
| **Each leaf**, one commit | New code reached only through the new feature |

Each commit carries the tests for what it adds. **Pause after the trunk:** ask the developer to say what changed and why without asking the agent; if they cannot, they read until they can. A use case with no trunk is one commit and no pause, except a security rule still goes alone. Fan out leaves to parallel worktrees only after the trunk merges, and no more agents than pull requests someone will review today. If the build finds a better way than the design, shout, and update `docs/design/<area>.md` in the same pull request.

### Step 4: Review against the contract

Use `templates/pr-and-code-review-checklist.md` (the course's six-item checklist, the triage line, and labeled comments). The author first runs a fresh-context review (`/code-review`, given the use case and the test list, since by default it reviews only the diff) and triages each finding: fix it, or reject it with a reason, recorded under How it was verified. The human reviewer writes the triage line, works the checklist, and approves when the change makes the code better than it was, never with a blocking comment open.

### Step 5: Close the use case (`/close-use-case`)

After the pull request merges and before the next use case starts (the `/close-use-case` command in `templates/claude-commands.md`): the traceability row honest about Build and Verify, with its Design, Code, and Tests cells filled; open findings logged as `OI-*`; actual hours summed from the weekly activity report lines that start with the use case ID; a **calibration ratio per estimate** = hours logged since that estimate was made ÷ the midpoint of its summed range; the owner's five-minute retrospective (Which part missed most? What did you not know when you estimated? What will you do differently on the next estimate?), its answer written as the use case's biggest-miss sentence in the team's calibration table that day; a charter rule for anything the agent had to be corrected on twice; the release plan revised. Then `/clear`.

The ratio corrects bias: at 1.5, multiply both ends of the next estimate by 1.5. The refined ratio corrects build estimates; the coarse ratio corrects the release forecast. Track it per person and per team. When the ratio says a promised date will slip, tell the client at the next meeting, not on the date. The course grades recording and interpreting estimates, never their accuracy.

## Gotchas

- **Silent design drift.** A working pull request that diverges from the design-of-record turns the design into a lie. The deviation is named under Reviewer focus and the design updated in the same pull request.
- **Logging a whole object.** `LOGGER.info("Saved {}", entity)` prints every field, emails, scores, and tokens included. Log identifiers and the failure reason; every `catch` that does not rethrow logs the exception.
- **Asking the agent to "add logging".** It will log everything, and the reviewer has to ask what was logged, not only whether.
