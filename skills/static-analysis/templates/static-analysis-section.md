<!-- Where the gate is written down. Paste each part into the document named in its heading, fill
     the _[brackets]_, and delete these instructions. Nothing here goes in the specification (it is
     not a stakeholder requirement) or in a KD-* (an analyzer can be swapped in an afternoon); cite
     the SEC-* or MNT-* attribute that asked for it, if one did. -->

## Architecture-of-record, §8.2: Static analysis

**The rule:** every change passes the gate before it merges: `_[the gate command, e.g. ./mvnw verify, or the script that runs each check]_`, the same command the pull request runs. _[Cites SEC-… or MNT-…, if one asked for it.]_

| Part | Formatter | Linter | Type checker | Why these |
|---|---|---|---|---|

_[Beyond the code, if adopted: dependencies, secrets, containers, dataflow, and where each runs.]_

**How a finding is settled:**

- **Real:** fixed, with a test when the bug can be shown.
- **False positive:** suppressed at the site with a reason a reviewer can check.
- **Not worth it:** suppressed with its reason, or the rule turned off in `_[configuration file]_` with the reason beside it.
- **Baseline:** `_[file, or "none"]_`. It only shrinks; an entry leaves when its finding is fixed or suppressed.
- **Custom rules:** in `_[file]_`, each with one example it must flag and one it must pass.

## Architecture-of-record, §7: where each check runs

| Where | What runs |
|---|---|
| Editor | _[inspections and plugins]_ |
| Before pushing | The gate command _[and any Git hook; a hook can be skipped, so it is never the only gate]_ |
| Pull request | `.github/workflows/pull-request.yml`, required on the main branch by the _[ruleset name]_ ruleset |
| On a schedule | _[Dependabot, CodeQL on the main branch]_ |
| Deploy | _[the same checks as the pull request; a deploy that skips one ships its findings]_ |

## Architecture-of-record, §11: debt rows

| ID | Type | What could go wrong, and what it breaks | Mitigation or fix | Cites |
|---|---|---|---|---|
| `TD-analyzer-baseline` | Debt | _[N SpotBugs, M PMD, … findings baselined untriaged, and the grain: for example, a new finding of a listed rule in a listed file passes]_ | _[Shrink by triage; owner]_ | §8.2 |
| `TD-gate-scope` | Debt | _[What the gate does not check: a platform build, a part with report-only checks]_ | _[When and how it joins the gate; owner]_ | §8.2 |

## AGENTS.md (the charter)

Under **How to run the tests**:

- The gate: `_[the gate command]_`. Run it before saying a change is done; passing tests alone is not passing the gate.

Under **The workflow the agent follows**:

- **The gate is the analyzers, and you answer to it** (architecture-of-record §8.2). Every suppression carries a reason a reviewer can check. A baseline only shrinks. Never weaken a check, suppress a finding, or add a baseline entry to make the build pass without saying so in the pull request.
