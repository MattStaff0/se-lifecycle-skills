---
name: Use case build context
about: "One issue per use case: cite the use case and its design, never copy them."
title: "UC-<AREA>-<slug>: <use case name>"
labels: use-case
---

<!-- Source: the course's Context Engineering module, 4.5 ("A build-context is an assembly, not a copy")
     and AI-Assisted Implementation 4.1-4.4 (two estimates on the issue, the four parts, calibration);
     Project Pulse docs/methodology.md, Phase B (github.com/Washingtonwei/project-pulse, Apache-2.0).
     Install as .github/ISSUE_TEMPLATE/use-case.md. Before the agent builds from this issue, run the
     questions test on it: a fresh session reads only this issue and what it cites, and lists what it
     would still have to guess. -->

## Build from
<!-- Pointers only. The use case already cites its business rules and quality attributes, so do not
     list them again here: a second list drifts like a pasted use case. -->
- Use case: `UC-<AREA>-<slug>` in `docs/requirements/use-cases.md`
- Design-of-record: `docs/design/<area>.md` (design PR #)

## Files it will touch
- `path/to/file` (new | reused)

## Context that lives nowhere else yet
<!-- The residue: a decision from this week's client meeting that has not reached the spec yet, why this
     use case is being built before that one. If a paragraph here belongs in the spec, write it in the
     spec and cite it instead. -->

## Coarse estimate
<!-- When the use case is written. The team, by planning poker. Ranges of human hours; people write
     these numbers, never the agent. -->
Made: YYYY-MM-DD, by the team

| Part | Low (h) | High (h) |
|---|---|---|
| Clarify | | |
| Design | | |
| Build and review | | |
| Verify | | |
| **Sum** | | |

## Refined estimate
<!-- After the design merges. The owner, sized from the design's test list. -->
Made: YYYY-MM-DD, by <owner> · Test-list rows: · Files touched: · Open risks:

| Part | Low (h) | High (h) |
|---|---|---|
| Clarify and design, what is left | | |
| Build and review | | |
| Verify | | |
| **Sum** | | |

## Close-out
<!-- After the pull request merges. Actual hours = the weekly activity report lines that start with this
     use case ID, counting only lines dated on or after each estimate's Made date for that estimate's
     ratio. Ratio = those hours ÷ ((sum of lows + sum of highs) ÷ 2). Copy the result and the
     biggest-miss sentence into the team's calibration table. -->
- Hours since the coarse estimate: · coarse ratio:
- Hours since the refined estimate: · refined ratio:
- Which part missed most?
- What did you not know when you estimated?
- What will you do differently on the next estimate?
