<!-- Install as .github/pull_request_template.md (or your host's equivalent). Title: one imperative
     line that still makes sense in the history. At most three lines per section, and never narrate
     the diff: if it won't fit on one screen, the pull request does more than one thing; split it. -->
Closes #
Traces to: <!-- slice IDs (UC-/DP-/MD-), BR-*, the design-of-record, or "none: refactor | config" -->

## Why
<!-- The problem in one or two sentences, readable without opening the issue. -->

## What changed
<!-- A summary, not a narration. Decisions the code does not show, and what you rejected.
     Anything you deliberately left alone. -->

## How it was verified
- **Verified:**
- **Not verified:**
<!-- Evidence, not assertion. Code: tests mapped to the design's test-list rows, what you ran end to
     end, a screenshot for UI. Data: the data tests per output, a rerun showing idempotency.
     ML: the evaluation against the baseline and each failure slice, on the held-out set.
     Spec or design: the source, what else you updated so the documents still agree, and the
     questions test in a collapsed <details> block (outside the length limit).
     Agent review: what a fresh-context review found and what you did about each finding.
     Not verified: what nobody checked. An honest answer here is the most useful line in the PR. -->

## Reviewer focus
<!-- Where to look hardest: which commit is the trunk, any security rule or migration, shared code,
     a rule or glossary term others depend on, and where to start reading. For a design: the
     decisions most likely to be wrong, and any change to the architecture-of-record. -->
