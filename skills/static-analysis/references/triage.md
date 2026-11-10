# Triage, suppressions, baselines, and custom rules

## Three verdicts, each recorded where the next reader will see it

A verdict that lives only in someone's head gets made again by the next person, or by the agent, which will happily "fix" a false positive.

| Verdict | Meaning | Recorded as |
|---|---|---|
| **Real** | The code is wrong, or will be | A fix, with a test when the bug can be shown |
| **False positive** | The code is right; the tool cannot see why | A suppression at the site, with the reason |
| **Not worth it** | True, but the fix costs more than the risk here | A suppression with the reason, or the rule turned off project-wide with the reason in the configuration |

**Questions that settle most findings:**

- **Is the tool configured for this code?** One rule flooding the report (test-framework globals reported as undefined, generated code, vendored files) is a configuration fix that removes all of them at once.
- **Is it one decision?** When the same verdict fits every finding of a rule (a framework that shares mutable objects on purpose, an empty constructor the ORM requires), make it once, in the configuration, not at every site.
- **Does the right fix depend on intent?** A value computed and never used may mean a line to delete or a missing return. The slice contract decides; if it is silent, it is an `OI-*`.
- **Is it true only in context?** "This can be null" may be false behind the authentication filter and true in a scheduled job. The reason names the assumption, which is also what a reviewer checks.
- **Is it deliberate?** An identity comparison that is correct here gets a suppression that says why, or the next agent "fixes" it and changes behavior.
- **"Not worth it" needs a reason about this codebase.** Without one, it is a false positive nobody checked.

**A suppression without a reason is a bug report you deleted.** Write a claim someone can check ("both entities come from one persistence context"), never a feeling ("false positive", "noise").

## How to suppress, with the reason attached

| Tool | At one site | For the whole project |
|---|---|---|
| SpotBugs | `@SuppressFBWarnings(value = "RULE", justification = "...")` | An exclude filter file, one `<Match>` per decision, with an XML comment |
| PMD | `// NOPMD - reason`, or `@SuppressWarnings("PMD.RuleName")` with a comment | An `<exclude>` in the team's ruleset file, with a comment |
| ESLint | `// eslint-disable-next-line rule-name -- reason` | `rules: { 'rule-name': 'off' }` in `eslint.config.js`, with a comment |
| Ruff | `# noqa: CODE  reason` | `ignore` or `per-file-ignores` in `pyproject.toml`, with a comment |
| mypy | `# type: ignore[code]  # reason` | A per-module override in the configuration, with a comment |
| TypeScript | `// @ts-expect-error reason` (fails when the error goes away, unlike `@ts-ignore`) | A compiler option, with a comment |
| Roslyn (.NET) | `#pragma warning disable CODE // reason`, or `[SuppressMessage(..., Justification = "...")]` | `.editorconfig` severity, with a comment |
| Semgrep | `// nosemgrep: rule-id reason` | Remove the rule from the ruleset, with a comment |

Check the tool's current documentation for the exact syntax; these change between major versions.

## An existing codebase: baseline, then ratchet

The first run on an existing codebase prints hundreds of findings, and waiting to fix them all means every day without a gate adds more.

1. **Record what exists today as a baseline**, untriaged.
2. **Fail the build only on findings outside it.** New code meets the bar from the first day; old code is not blocked.
3. **Shrink it:** fix or suppress an old finding (with its verdict), then delete its entry. **Never add an entry to make a build pass.** The file only gets smaller; that is the ratchet.

**Keep the baseline apart from the decisions.** Rules the team turned off on purpose live in the configuration with their reasons; the baseline is "not looked at yet". Mixing them lets "we decided" hide inside "we have not looked". Open the baseline file with a comment that it only shrinks.

**Where a tool keeps its baseline** (check the current docs, because these change):

| Tool | Baseline mechanism | Grain |
|---|---|---|
| SpotBugs | A second exclude filter file, one `<Match>` per finding (class, method, rule) | Method |
| PMD (Maven) | `excludeFromFailureFile`: one line per class listing the rules it may break | Class: a new finding of a listed rule in that class passes |
| ESLint 9 | Bulk suppressions (`--suppress-all` writes `eslint-suppressions.json`) | Rule count per file |
| detekt, Android Lint | A generated baseline file | Finding |
| Ruff | `ruff check --add-noqa="baseline: untriaged"` writes `# noqa: CODE baseline: untriaged` on each finding's line, so the baseline is one `grep` away; or a `per-file-ignores` block marked as the baseline | Line, or file |
| No native mechanism | `per-file-ignores`, or overrides in a block marked as the baseline, or the tool's SARIF compared against a committed list | Usually file |

**Know your grain, and say it in the §11 debt row.** A finer grain leaks less; a coarser one is easier to keep.

**Upgrading a tool can break its baseline.** A rule renamed between versions brings its baselined findings back as new. Upgrade an analyzer in its own pull request, and compare counts by rule before and after.

**Format in one commit.** Reformat the whole codebase alone, early, and list that commit in `.git-blame-ignore-revs` so `git blame` skips it. Then the formatter's check joins the gate.

## Your own rules

Shipped rules know common mistakes; they do not know the team's conventions, which live in §8.2 of the architecture-of-record and in review comments. When a reviewer writes the same comment the third time, the comment is a rule waiting to be written.

| Tool | Writes the rule as | Good for |
|---|---|---|
| Semgrep | A pattern that looks like the code, with `$X` placeholders | Most languages; readable; quick to try |
| PMD | An XPath query over the Java syntax tree | Java conventions, in the existing build |
| ESLint `no-restricted-syntax` | An AST selector in the configuration | Frontend conventions, no plugin needed |
| ArchUnit, import-linter, dependency-cruiser | A test over classes, modules, or imports | Architecture rules |

**A rule is code, so test it both ways.** Keep one example it must flag and one it must pass, committed beside it (Semgrep's `ruleid:` and `ok:` annotations with `semgrep --test`, PMD's rule tests, or a small fixture the build runs). Without the passing example you cannot tell a rule that works from one that matches everything; without the flagging one, from one that matches nothing. Claude drafts the rule; the team chooses the convention and writes or reads the two examples, because they are the rule's specification.

**Your own rules need triage too.** A broad first draft ("every catch that does not rethrow must log") will flag code that is right on purpose. Narrow the rule, or suppress that site with its reason.

## What the agent may and may not decide

- **May:** explain a rule and why it fired; group a report by rule; fix findings with an obvious fix once the team has agreed the rule applies (an unused import, a missing locale argument, the formatter's output); draft the configuration, the baseline, the workflow, and a custom rule; write the test that shows a real finding is a bug.
- **May not, without a person's yes in the pull request:** add a baseline entry; suppress a finding; turn a rule off; weaken a check (`continue-on-error`, a lower severity, a narrower path); change behavior to quiet a warning.
- **Reviewing the agent's work:** run the gate yourself; read every suppression and baseline change in the diff as code; for each "fixed" finding, check the fix is the right one and not the quietest one.
