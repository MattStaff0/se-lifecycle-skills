---
name: static-analysis
description: Use when adding static analysis to a project (linters, formatters, type checkers, SpotBugs, PMD, ESLint, Ruff, CodeQL, Semgrep, Dependabot, secret scanning), setting up a pull request gate or CI checks that block a merge, triaging analyzer findings or a noisy first run, creating or shrinking a baseline, suppressing a warning, turning a repeated review comment into a custom lint rule, or reviewing an agent's suppressions or baseline edits.
---

# Static Analysis and the Gate

Work with the team to put analyzers on the project and make them a **gate**: the checks every change must pass before it can merge, the team's code and the agent's alike. The analyzer finds, the agent fixes, the analyzer checks again, and a person decides only what needs judgment. On a new project the gate goes on with the proving slice, before the first feature merges; on an existing one, it goes on with a baseline that only shrinks. It sits between the architecture and the slice cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder). `references/ladder.md` lists what each rung catches, candidate tools per stack, and how to count findings by rule; `references/triage.md` covers verdicts, suppressions, baselines, and custom rules.

**Why it works this way:** whether a program can ever do a given thing is undecidable (Rice's theorem), so every analyzer approximates and has false positives, false negatives, or both. A clean report is not a proof, and a team stops reading a tool that cries wolf, so every rule turned on must pay for the attention it costs.

## Templates

- **The project's own template wins.** If `AGENTS.md`'s Documents table names a template source for the gate (a team's, a course's), use it: for a git clone, `git -C <clone> fetch` first and pull only on a yes; where it and this skill disagree, the template wins, and say so.
- Otherwise use `templates/pull-request-workflow.yml` (the GitHub Actions workflow), `templates/gate-pull-request.md` (the gate pull request's description), and `templates/static-analysis-section.md` (the architecture-of-record subsection, its §7 and §11 rows, and the charter lines). Copy with `cp -n`, on a branch, and fill underneath the instructions.

## Standing rules

- **A check that does not fail the build is decoration.** Every adopted check runs in the gate, and the gate is required on the main branch.
- **Check in the gate, fix on the machine.** CI runs checking modes only: no `--fix`, no `--write`, no `continue-on-error` (outside the temporary warn-first stage in Step 5). Fixing modes run locally.
- **One gate command.** The command a person or the agent runs before pushing is the one the pull request runs. Green tests are not a green gate.
- **Read by rule before by file.** The big rules are usually one decision each; the bugs hide in the small ones.
- **The verdicts are human.** Claude fixes findings whose fix is obvious once the team agrees the rule applies. Every suppression, rule turned off, and baseline entry kept is a claim someone must defend, so a person makes it. Never quiet a finding by changing behavior (an identity `==` "fixed" to `.equals()`).
- **Baselines only shrink.** Never add a baseline entry, suppress a finding, or weaken a check to turn a build green without saying so in the pull request.
- **Every suppression carries a checkable reason**: a claim a reviewer can verify ("only called behind the authentication filter"), never "false positive".
- **Tool facts are looked up, never remembered.** Latest release dates come from the registry or the project's releases page; action SHAs from `gh api repos/<owner>/<repo>/commits/<tag> --jq .sha`.
- **Coach.** Before each step, say in one or two sentences what it is for and what the team decides in it.
- **Git and settings:** work on a branch; propose commits, the push, and pull requests, and make them only on a yes. Repository settings (rulesets, Dependabot, code and secret scanning) are changed by a person; Claude gives the steps.

## Workflow

### Step 1: Choose the tools (a proposal)

Read `AGENTS.md` (project types, run and test commands), the architecture-of-record (containers and their technology, §7, §8.2), and the existing build and CI files. Note every analyzer that is installed but never runs, and every deploy path that skips a check: those are findings already. For each part of the stack, propose from `references/ladder.md` a formatter, a linter, and a type checker if the language has one, then the rungs beyond your own code as they apply (dependencies, secrets, containers, dataflow). For each tool, give what it catches, why it beats the alternatives, its latest release date (looked up), and its checking-mode command. Hold every candidate to the four tests: free and runs in the project's own build with no account; maintained; has a checking mode; one tool per rung.

**STOP.** The team chooses.

### Step 2: The first run, without touching the build

Run each chosen tool in checking mode without editing build files (a plugin by its coordinates, `npx`, `pipx run`). Count findings by rule (`references/ladder.md`, "Counting by rule") and report one table per tool: rule, what it means, count, first move (one project decision, read each, read one, or a configuration problem). Say which ruleset ran: a default ruleset can report a fraction of what the configured one will. When one rule floods the report, check first whether the tool is configured for this codebase (test globals, generated code, notebooks). **Fix nothing.**

**STOP.** The team reads the counts.

### Step 3: Triage

Rule by rule, with `references/triage.md`: the team decides which rules to turn off for the whole project, with the reason in the configuration. Each remaining finding gets one of three verdicts: **real** (a fix, with a test when the bug can be shown), **false positive** (a suppression at the site with its reason), or **not worth it** (a suppression, or the rule off, with a reason that says what makes the risk small in this codebase). A real finding whose right fix depends on intent goes to the slice contract to decide, or becomes an `OI-*`. Claude drafts each verdict with its evidence, marked as a proposal; a person accepts every verdict that is not "fix it".

### Step 4: Wire the gate

- **A new project:** triage everything and skip the baseline; from then on, every finding fails the build.
- **An existing codebase:** baseline what is still untriaged, in a file of its own, apart from the decisions, opened with a comment that it only shrinks; say its grain (`references/triage.md`).

Bind the checks into the build so the gate command runs them, then write the workflow from `templates/pull-request-workflow.yml`: on every pull request to the main branch, one job per part of the stack, running the build, the tests, and every chosen check; checking modes only; every action pinned to a full commit SHA with its version in a comment; `working-directory` for a part in a subfolder; an ordinary Linux runner. A device or platform build is not part of the first gate; if the gate leaves it out, record that as a §11 debt row. If the formatter would change many files, reformat in one commit of its own and list it in `.git-blame-ignore-revs`.

Explain the workflow line by line, then draft the pull request description from `templates/gate-pull-request.md`: the first-run counts, the decisions, the baseline, and the comparison table (one row per job: what fails it, and what it does not check). **The team corrects the table**, because "what fails it" and "what it does not check" are where their review shows.

**STOP.** The team reads every line against the template's workflow questions, and a teammate approves before it merges. If the workflow goes red on its own pull request, that is the first run telling the truth: fix the easy findings, turn off rejected rules with reasons, baseline the rest, and get the main branch green before Step 5. "Unable to resolve action" means a made-up SHA: look it up.

### Step 5: Make red block, and watch it fail

After the workflow has merged and run once (a check can be required only after it has reported), a person creates a ruleset on the main branch: require a pull request, require the gate's checks by job name, block force pushes and deletion. Then the team tries a direct push to the main branch and sees it refused; plants one bug the tools must catch on a branch, opens a pull request, and sees it go red with the merge blocked; then closes that pull request unmerged. **A gate nobody has seen fail is only hoped to work.**

A check with too many existing findings may run report-only (`continue-on-error: true`) for a short time, with a §11 debt row and an owner, until its findings are fixed or baselined. A warning nobody must act on is soon a warning nobody reads.

### Step 6: Write it down

From `templates/static-analysis-section.md`: a **Static analysis** subsection in §8.2 of the architecture-of-record (the tools and why, the gate command, how a finding is settled and where each verdict is recorded); in §7, where each check runs; in §11, a debt row for any baseline (counts by tool, and its grain) and for anything the gate leaves out; and the charter lines in `AGENTS.md`, citing §8.2, with the gate command under "How to run the tests". It goes nowhere else: it is not a stakeholder requirement, so not the spec (cite a `SEC-*` or `MNT-*` if one asked for it), and not a `KD-*`, because an analyzer can be swapped in an afternoon.

### Later, on demand

- **A custom rule** when a review comment comes back the third time: Claude drafts it; the team chooses the convention and writes or reads two examples, one the rule must flag and one it must pass, committed as its test. Its findings get triaged like any rule's (`references/triage.md`).
- **The higher rungs:** null types one package at a time, dataflow (CodeQL, Semgrep), Dependabot once CI would catch a broken update (including the `github-actions` ecosystem, which keeps pinned SHAs current), secret scanning with push protection, an image scanner.
- **Upgrade an analyzer** in its own pull request, comparing counts by rule before and after: a renamed rule brings baselined findings back as new. An end-of-life analyzer is itself a finding.
- **Remove a rule** whose findings nobody acts on. Google's bar: when developers ignore more than 1 in 10 of a check's warnings, the check is fixed or turned off.

## Gotchas

- **Installed is not running.** Ask "what fails the build?", never "what is installed?", and make the deploy run the same checks as the pull request.
- **A later build phase.** Where the analyzers bind to a phase after the tests (Maven's `verify`), a passing `test` says nothing about them.
- **The agent's tool list is a claim.** It can recommend an end-of-life version or invent a SHA; the first run's counts by rule are the evidence.
- **A coarse baseline leaks.** A baseline keyed by file or class lets a new finding of an already-listed rule in that file pass. Prefer the finest grain the tool offers, and say which you have.
- **A clean report can miss the point.** A linter cannot see an instruction that is missing (a Dockerfile with no `USER`); cover each rung with a tool that can see it.
- **An AI review is not the gate.** A model can answer differently each run and can be argued past; it reviews beside the analyzer, never instead of it.
- **Data and ML:** notebooks and SQL are code, so they get a linter and a formatter too; committed notebook output is stripped. Data-quality checks belong to the data contract's tests, not here: static analysis reads the code, never the data.
