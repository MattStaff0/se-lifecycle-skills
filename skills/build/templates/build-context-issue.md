---
name: Slice build context
about: "One issue per slice: cite the contract and its design, never copy them."
title: "<slice ID>: <slice name>"
labels: slice
---

<!-- Install as .github/ISSUE_TEMPLATE/slice.md (or paste into your tracker: Jira, Linear, ...).
     A build-context is an assembly of pointers, not a copy: a pasted contract is a second contract
     that drifts. Before the agent builds from this issue, run the questions test on it: a fresh
     session reads only this issue and what it cites, and lists what it would still have to guess. -->

## Build from
<!-- Pointers only. The contract already cites its rules and quality attributes; don't list them again. -->
- Slice contract: `<UC-|DP-|MD-><AREA>-<slug>`
- Design-of-record: `docs/design/<area>.md` (design PR #)

## Files it will touch
- `path/to/file` (new | reused)

## Context that lives nowhere else yet
<!-- The residue: a decision from this week's conversation that hasn't reached the spec yet, why this
     slice comes before that one. If a paragraph here belongs in the spec, write it there and cite it. -->

## Coarse estimate
<!-- When the slice is written. The team, by planning poker. Ranges of human hours; people write
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
<!-- After the pull request merges. Actual hours = the time-log entries that start with this slice
     ID, counting for each estimate only entries dated on or after its Made date.
     Ratio = those hours ÷ ((sum of lows + sum of highs) ÷ 2). Copy the result and the biggest-miss
     sentence into the calibration table. -->
- Hours since the coarse estimate: · coarse ratio:
- Hours since the refined estimate: · refined ratio:
- Which part missed most?
- What did you not know when you estimated?
- What will you do differently next time?
