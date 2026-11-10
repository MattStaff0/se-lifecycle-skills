<!-- The description of the pull request that adds the gate. Claude drafts it; the team corrects it,
     above all the comparison table's last two columns, which are where the team's review shows.
     Fill underneath each heading and keep every section short. -->

## What this gate checks

_[One sentence: on every pull request to the main branch, these jobs must pass before it can merge.]_

## Tools chosen

| Part of the stack | Tool | Rung | Why this one | Latest release (checked on) |
|---|---|---|---|---|

## First run, by rule

_[From the checking-mode run, before anything was fixed. Say which ruleset or configuration ran.]_

| Tool | Rule | Count | What the team decided |
|---|---|---|---|

## Rules turned off, and why

_[Each with its reason, which also sits beside the exclusion in the configuration file. "None" is a fine answer.]_

## Baseline

_[None, because the project is new and everything was triaged. Or: the file, the counts by tool, its grain, and the §11 debt row that tracks it.]_

## Comparison table

| Job | Command | What fails it | What it does not check |
|---|---|---|---|

## Reviewing the workflow

_[The team answers each before a teammate approves.]_

1. **When does it run?** Which events and branches trigger it.
2. **Is it the gate command?** Each job runs what a person runs before pushing, not a lighter version.
3. **What fails it?** Every step can fail the job: no `continue-on-error`, no `|| true`, no fixing mode.
4. **What does it not check?** A part of the stack with no job, a platform build left out (a §11 debt row).
5. **What third-party code runs?** Every action is pinned to a full commit SHA that was looked up.
6. **What can it reach?** `permissions:` as narrow as the job needs, which secrets it sees, and no `pull_request_target`.

## How it was verified

- [ ] The gate command run locally: _[result]_
- [ ] This pull request's checks: _[green, or what went red and how it was settled]_
- [ ] After the merge: a direct push to the main branch refused, and a planted-bug pull request red with its merge blocked, then closed unmerged
