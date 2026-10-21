# Code Review Checklist

<!-- Source: the course's Code Review module (https://washingtonwei.github.io/cosc-40943/modules/code-review/),
     4.3 (triage depth), 4.4 (the checklist), 4.5 (writing the review), 4.6 (the author's half), and
     4.7 (the agent as first reviewer). If that page has changed, the page wins. The pull request
     description itself follows the course's .github/PULL_REQUEST_TEMPLATE.md. -->

Review against the contract: the use case with every extension, its business rules, and the design-of-record's API contract and test list. A diff shows what was added, never what was left out.

## How deep: the triage line

| Commit | Depth |
|---|---|
| **Trunk**: code many paths depend on, that changes behavior users rely on, or that is hard to undo | Every line |
| **Security rule or migration** | Every line, and where it sits |
| **Leaf**: new code reached only through the new feature | Its tests' assertions against the test list; skim the rest |

Write the triage into the review, one line: "Trunk: `<class>`, read in full. Security: `<rule>`, read in full. Leaves: `<controller and view>`, checked tests against rows 1 to N, skimmed the rest." A skim you wrote down is a decision a teammate can challenge; a skim you did not is a rubber stamp.

## The checklist

1. **The test list.** Each row: covered by a test, or named as missing.
2. **The design.** Classes, calls, routes, and decisions match the design-of-record; every deviation is named and the design updated in the same pull request.
3. **The triage line.** Trunk and security commits read in full.
4. **The logic against the description.** Does the code do what the pull request says, and what the use case says?
5. **Each extension's error path.** What happens when it fails, and does every `catch` that does not rethrow log the exception?
6. **The tests check the contract.** An expected value taken from the use case, not echoed from a mock.

Style, formatting, unused variables, and duplication are the linter's job from week 10; until then, a "Nit:" comment.

## Writing the review

Label every comment (Conventional Comments): `issue`, `suggestion`, `question`, `nitpick`, `praise`, each marked blocking or non-blocking.

```text
issue (blocking): test-list row "UC-ORD 4a" has no test. What does POST /orders do for a sold-out item?
nitpick (non-blocking): `cnt` → `sentCount`.
```

Approve when the change makes the code better than it was, even if not perfect. Never approve with a blocking comment open. Merging is a person's decision.

## The author's half

- Review your own pull request before filing it, including a description the agent drafted.
- Fill the four sections of `.github/PULL_REQUEST_TEMPLATE.md`: Why; What changed; How it was verified (**Verified:** tests mapped to test-list rows, what you ran end to end, a screenshot for a UI change; **Not verified:** what nobody checked); Reviewer focus (which commit is the trunk, any security rule, where to start). A self-rated confidence is not evidence.
- Run a fresh-context review first: `/code-review <PR>`, **naming the use case and the design's test list**, because by default it reviews the diff and will not find a missing extension. Triage each finding like a teammate's: fix it, or reject it with a reason, recorded under How it was verified. Spot-check one thing it flagged and one it passed. If its review never mentions the test list, it reviewed the diff.

## After the review

If the agent had to be corrected on the same thing more than once, turn it into a rule in `AGENTS.md` with a small pull request of its own. Where the rule applies to code already merged, open an issue for that code rather than widening this pull request.
