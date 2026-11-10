# Review Checklist

Review against the **contract**, not just the diff: the slice (with every failure path), the rules it cites, and the design-of-record's contract and test list. A diff shows what was added; it never shows what was left out.

## Set the depth: the triage line

| Commit | How deep |
|---|---|
| **Trunk**: code many paths depend on, that changes behavior people rely on, or that is hard to undo | Every line |
| **Security rule, migration, schema change, permissions grant** | Every line, and where it sits |
| **Leaf**: new code reached only through this slice | Its tests' assertions against the test list; skim the rest |

Write it as the first line of the review, so the depth is a decision a teammate can challenge, not a silent skim:

> Triage: trunk `<sha>` read in full; migration `<sha>` read in full; leaves `<sha>…` checked against test-list rows 1–N, rest skimmed.

## Six checks

1. **Every test-list row** is covered by a named test (or data test, or evaluation check), or called out as missing.
2. **The design matches.** Components, calls, stages, and decisions are the ones the design-of-record names; every deviation is named in the description and the design is updated in the same pull request.
3. **The triage held.** The trunk and security commits were read in full.
4. **The code does what the description and the contract say**, not just what the tests check.
5. **Every failure path behaves as specified**, and every exception handler that does not rethrow logs the exception (identifiers and reason, never content).
6. **Expected values come from the contract**, not echoed from a mock or from the code's own current output.

## By project type

- **Data:** every output has its data tests; a rerun produces the same result; a late or malformed input does what the contract says (stop, quarantine, flag); no personal data in logs or fixtures.
- **ML:** evaluation runs on the held-out set only (no test-set tuning, no leakage across the split); the model beats the baseline overall **and** on every named failure slice; the fallback is tested; the run is reproducible (versions, seeds, data snapshot); no notebook code was merged as production.

Style, formatting, and unused code are the linter's job; leave them to a non-blocking nit. The gate is green before review starts, and every suppression or baseline change in the diff is read as code: its reason is a claim you can check, and a baseline only shrinks.

## Write the review

Label every comment and mark it blocking or not: `issue`, `suggestion`, `question`, `nitpick`, `praise` (the Conventional Comments format).

```text
issue (blocking): test-list row "DP-FIN-daily-revenue 1a" has no test. What happens when the orders file is late?
nitpick (non-blocking): `cnt` → `row_count`.
```

Approve when the change leaves the code better than it was, even if not perfect. Never approve with a blocking comment open. Merging is a person's decision.

## The author's half

- Review your own pull request first, including a description the agent drafted.
- Fill the PR template honestly; **Not verified** is the most useful line in it.
- Run a fresh-context agent review first (`/code-review`, **naming the slice and the design's test list**; by default it reviews only the diff and will not find a missing failure path). For each finding: fix it, or reject it with a reason, recorded under How it was verified. Spot-check one thing it flagged and one thing it passed.

## After the review

If the agent had to be corrected on the same thing more than once, turn it into a rule in `AGENTS.md` with its own small pull request. If the rule applies to code already merged, open an issue for that code rather than widening this pull request.
