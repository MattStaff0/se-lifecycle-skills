# Slash commands: `/estimate`, `/implement`, `/close-use-case`

Copy each block below into the target repository as `.claude/skills/<name>/SKILL.md` (the course's current form; `.claude/commands/<name>.md`, the older form, also works). Commit them by pull request, since the whole team runs them. The course suggests these three (AI-Assisted Implementation 4.11) and follows one skeleton: front matter, the argument, preconditions, standing rules, phases that read first, stops and handoff.

| Command | Source |
|---|---|
| `/implement` | Adapted from Project Pulse's `.claude/commands/implement.md` (github.com/Washingtonwei/project-pulse, Apache-2.0). Its Spring, Vue, and RAM specifics are replaced by pointers to the repository's own architecture-of-record §8; the guard-rail preconditions and the push-on-a-yes gate are added. |
| `/estimate` | The course's description in AI-Assisted Implementation 4.1–4.2 and 4.11 ("count the design's test rows, the files its sequence diagrams touch, and its open risks, and draft the refined breakdown with the hours left blank"). Pulse has no such command. |
| `/close-use-case` | The course's closing checklist (Method, Phase C step 4) and the retrospective of AI-Assisted Implementation 4.4. Pulse has no such command. |

---

## `.claude/skills/estimate/SKILL.md`

````markdown
---
description: "Prepare a use case's estimate: count from the use case or design, leave the hours blank for people."
argument-hint: "<UC-ID> [coarse|refined]"
---

# /estimate: count, never guess

Prepare the estimate for the use case in `$ARGUMENTS`. **You count; people write the hours.** Never write an hour number, a suggested range, or a "typical" figure, even when asked: say the hours are the owner's call, and why (only they know how fast they read a diff and how long the client takes to answer).

## Preconditions

- `coarse`: the use case is written. `refined`: its design-of-record has merged (`docs/design/<area>.md` covers it). If the mode in `$ARGUMENTS` contradicts the repository, say so.

## Phases

1. **Read.** The use case in `docs/requirements/use-cases.md` (Grep its ID; never read the file whole), its open `OI-*` items, its traceability row, and for `refined` the design-of-record.
2. **Count.**
   - `coarse`: main-scenario steps, extensions, cited business rules, open `OI-*` items, external interfaces crossed.
   - `refined`: test-list rows by level, files touched (new or reused), whether there is a trunk, a separate security-rule or migration commit, and the open risks in the design (a risk settled only by measuring widens the high end).
3. **Present** the table with Low and High blank, each count beside the part it informs. Coarse has four parts (Clarify, Design, Build and review, Verify), made by the team in planning poker. Refined has three (Clarify and design, what is left; Build and review; Verify), made by the owner. If closed use cases carry ratios, show the matching one (coarse ratio for coarse, refined for refined) and the arithmetic: both ends × ratio.
4. **Offer** to add it to the use case's issue with today's date as its Made date. Draft the edit; post it only on a yes. If a range looks too small for the counts, say so: cut scope, never the estimate.
````

---

## `.claude/skills/implement/SKILL.md`

````markdown
---
description: "Implement a use case from its merged design-of-record: plan, then trunk commit, pause, leaf commits, tests, traceability, pull request draft. Run /design first."
argument-hint: "<UC-ID>"
disable-model-invocation: true
---

# /implement: build a use case from its design

Take the use case in `$ARGUMENTS` from its design-of-record to working, tested code. The use case is the contract; its linked FRs are the acceptance criteria; the glossary fixes the vocabulary; `docs/design/<area>.md` is how you build it.

## Preconditions (refuse to start)

- **The design has merged.** `docs/design/<area>.md` covers this use case (its Realizes header lists the ID). If not, stop and tell the developer to run `/design <UC-ID>` first. Never design inside the build.
- **A fresh session.** The build reads the merged design, not the conversation that produced it. If this session carries the design conversation or other work, ask for `/clear`. The session then continues across the trunk pause: clearing happens between gates, not within one. Anything the documents do not answer, but that conversation did, is a gap in the design: ask, then fix the design in this branch and name the change under Reviewer focus.
- **The guard rails are on.** `.claude/settings.json` denies reading secrets, and `AGENTS.md` has the logging rule and the dependency rule. If not, say so before writing code.

## Standing rules

- **The spec and the design are authoritative but not infallible: challenge them.** Ambiguous step, contradiction, stale constraint, a better way: ask or push back; never silently comply or silently invent. When in doubt, or when you see a better way, shout.
- **No dependency** until a person confirms it exists and is the one intended.
- **Log identifiers, never content**; every `catch` that does not rethrow logs the exception.
- **Propose every commit; push and open the pull request only on a yes.** Never remove the `Co-Authored-By` trailer.

If no ID is given, list the use cases whose Build is `📐 Designed` in `docs/traceability.md`, in Release order, and ask which one.

## Phase 1: Read (no edits)

1. The design-of-record: its diagrams, components, decisions, API contract, and test list.
2. The use case (Grep its ID): actors, preconditions, main success scenario, extensions, business rules. These are the flows the tests cover.
3. The non-use-case FRs it depends on and its glossary terms.
4. §8 of `docs/design/architectural-design.md` in full. Where existing code breaks a convention, follow the convention and note the drift.
5. The traceability row (is any of it built?) and the existing code the design marks reused.

If the design disagrees with the spec or the code, flag it now and fix the design (or raise a spec defect) before building on it.

## Phase 2: Plan (no approval gate)

State a short plan and continue: acceptance criteria (the main flow and every extension), the trunk, any security-rule or migration commit, the leaves, the tests mapped to the design's test-list rows, any step relying on cross-cutting behavior with no backing FR, and any deviation from the design. A true blocker stops you here.

## Phase 3: Implement

On a branch, never `main`, in commits a reviewer can read, cut along the class diagram:

- **Trunk first, alone:** code many paths depend on, code that changes behavior existing users rely on, code that is hard to roll back. Then **stop**: name the commit to read and ask the developer to say what changed and why without asking you. Continue when they can, and when they have approved any new dependency.
- **A security rule or a schema migration, alone.**
- **Then one commit per leaf:** each flow's own controller, view, or job.
- **Each commit carries the tests for what it adds** and passes them, run as targeted tests.
- No trunk? One commit, no pause; a security rule or migration still goes alone.

If the build forces a change from the design, update `docs/design/<area>.md` to match.

## Phase 4: Test and verify

1. Check the diff against §8 before running anything: every endpoint has a route rule **and** a scoped query, with a test for each refusal; ids in request bodies re-resolved through a scoped lookup; request bodies validated where §8.2 says; caller identity from the shared helper; calendar time from the injected clock; logs carry identifiers only; a schema change has its migration. Fix a miss or record it as a deliberate deviation.
2. Run the full suites (commands in `AGENTS.md`); the developer may run them if they are slow. Report the result either way.
3. Where practical, run the main success scenario in the app.
4. Report what passed, what was skipped, and why.

## Phase 5: Trace it back

In the use case's `docs/traceability.md` row: Build `🟡 In progress` or `✅ Built`; Verify from the tests that exist (`✅ Verified` only if the main flow and every extension are tested and pass; `🟡 Partial` naming the gap; `🔎 None`); fill the Design, Code, and Tests cells, noting which extensions each test covers.

## Phase 6: Draft the pull request

Keep the commits separate (merge with a merge commit, not a squash, so trunk and leaves stay readable on `main`). Draft the description from `.github/PULL_REQUEST_TEMPLATE.md` (copy it from the course templates if the repository lacks it), at most three lines per section: `Closes #<build-context issue>`; Traces to the use case and `docs/design/<area>.md`; Verified (tests mapped to test-list rows, what Phase 4 ran) and Not verified (every untested extension, anything skipped); Reviewer focus (the trunk commit, every change to shared code, any security rule first, any design change). Recommend a fresh-context `/code-review` naming the use case and test list. **Push and open it only on the developer's yes.** After it merges, `/close-use-case <UC-ID>`.
````

---

## `.claude/skills/close-use-case/SKILL.md`

````markdown
---
description: "Close a merged use case: honest traceability, actual hours vs. each estimate, the retrospective, charter rules, the release plan."
argument-hint: "<UC-ID>"
disable-model-invocation: true
---

# /close-use-case: close it before the next one starts

Close the use case in `$ARGUMENTS` after its pull request merges. Draft every edit; commit, push, post, or delete nothing without the developer's yes. The owner answers the retrospective; never answer it for them.

## Preconditions

- The implementation pull request has merged. If not, stop.

## Phases

1. **Branch.** Offer to delete the merged branch; delete only on a yes.
2. **Traceability.** The row's Build and Verify match the tests that exist; Design, Code, and Tests cells filled; the Tests cell names the extensions covered. Fix it if not.
3. **Nothing floating.** Spec and architecture fixes the work surfaced have merged; anything still open is an `OI-*`.
4. **Hours.** Ask where the weekly activity reports are (or for the lines to be pasted). Sum the lines starting with this use case ID. For each estimate on the issue, count only lines dated on or after its Made date: ratio = those hours ÷ ((sum of lows + sum of highs) ÷ 2). Show the arithmetic.
5. **Retrospective (five minutes).** Ask the owner: Which part missed most? What did you not know when you estimated? What will you do differently on the next estimate? Draft their answer as this use case's biggest-miss sentence in the team's calibration table (ask where it lives the first time), beside its estimates, hours, and ratios.
6. **Charter.** Ask the reviewer what the agent had to be corrected on more than once; propose each as a rule in `AGENTS.md`, in its own small pull request. Where the rule applies to merged code, draft an issue for that code.
7. **Release plan.** With the ratios applied, do the remaining estimates still fit the client's date, and is the next use case still the right one? Propose any Release changes as a pull request that says why. If a promised date will slip, the client hears it at the next meeting.
8. Tell the developer to `/clear` before the next use case.
````
