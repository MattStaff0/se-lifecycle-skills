---
name: se-new-repo-orchestrator
description: Use when starting a brand-new repository or greenfield project, when unsure which spec-driven lifecycle document or step comes next, before prompting an agent to write specs or code on a new project (Napkin Drill), or when auditing a project against a course checkpoint (Checkpoint 0, 1, 2, or 3).
---

# SE New Repo Orchestrator

Find out where a project is in the course lifecycle, say what comes next and why, and hand off to the skill for that stage. Never let the agent invent requirements, architecture, or answers the client has not given. The full map, with weeks, checkpoints, and gates, is `references/COURSE-LIFECYCLE.md`; read it first.

## Standing rules

- **The repository is the only memory.** A decision not committed to the spec, the architecture, or `AGENTS.md` does not exist for the agent.
- **Cite, never copy.** Requirements and decisions are cited by slug ID (`FEAT-*`, `UC-<AREA>-*`, `BR-*`, `SEC-*`, `KD-*`); only `OI-1` upward is numbered.
- **Never answer for the client.** A missing policy or number becomes an `OI-*` in `docs/requirements/OPEN-ISSUES.md`.
- **Where a skill stops is its most important line.** Stop at every gate in the lifecycle map's "Gates where the human decides".
- **Coach.** Say in one or two sentences what each stage is for and what the developer decides in it. The developer is learning the workflow, not just filling documents.
- **Git:** work on a branch; propose commits and pull requests, and make them only on a yes.

## Step 1: Diagnose (read only)

Look at the repository and today's date. Check for: `docs/napkin-round-0.md`, `docs/team-contract.md` (and one signature commit per member), `AGENTS.md` with a `CLAUDE.md` whose first line is `@AGENTS.md`, `.claude/settings.json`, each file in `docs/requirements/` (and how far each is filled beneath its template instructions), `docs/traceability.md` (§1, §2, §3), `docs/design/architectural-design.md` (which sections), any `docs/design/<area>.md`, and tags (`v0.1.0`, `v0.2.0`). Then report three things:

1. **The stage the repository has reached**, from the lifecycle map's week-by-week table.
2. **What the course expects by now** for today's date, and the next checkpoint with what it checks.
3. **The gap between them**, as a short ordered list, each item naming the skill that handles it.

**STOP.** Wait for the developer to confirm the stage and pick what to work on. When a stage's skill finishes, come back and rerun this step, so the next step is named from the repository's state, not from memory.

## Step 2: Run the chosen stage

| Stage | Hand off to |
|---|---|
| The Napkin (before any spec or agent plan) | Run it here: below |
| Team contract, charter, settings | `se-charter-and-governance` |
| Interview guide, glossary, vision and scope, open issues; business rules | `se-requirements-foundation` |
| Use cases, SRS, traceability | `se-use-cases-and-srs` |
| Architecture-of-record | `se-architecture-of-record` |
| Pick the proving slice | Here: below |
| The slice's design, then build, review, close | `se-design-of-record`, then `se-slice-orchestrator` |

**Copying the course templates.** Each stage's skill copies the templates it needs, when it needs them, with no-clobber and after a `git fetch` of the course clone. To set up `docs/` in one go instead, use the `/new-repo-setup` command in `references/CLAUDE-COMMAND.md`.

### The Napkin

Use the six prompts and the fixed sequence in the lifecycle map (Shape, the hard part, Bottleneck, Stack, Kill risks, Verdict). **Ask each person to write their own napkin silently first, then reconcile as a team. Stop until they say they have.** Only then ask for the real constraints (budget, incumbent systems, deadline, who operates it afterwards, the team's skills, the definition of done), draft the agent's napkin, and run the diff with them in both directions. Reject any kill risk stated as a category ("integration issues") and ask for the mechanism ("the client's system exports CSV nightly, so the real-time dashboard is dead on arrival"). Round 0 on the team's own project is sealed in `docs/napkin-round-0.md` (one heading per prompt works: the team's reconciled answer, then the agent's, then what the diff found).

### Picking the proving slice

The proving slice is the use case that exercises the highest-ranked rows of the architecture's §9.1, crosses the trust boundary, or calls the riskiest external system: never a login screen or a simple list. Propose candidates with the ASRs each one exercises; **the team picks**. Then hand off to `se-design-of-record`.

## Gotchas

- **Prompting before thinking.** Never let the silent human step be skipped, in the Napkin or the design sketch; the agent's answer seen first anchors everyone on it.
- **Architecture by feature list.** No microservices, queues, or Kubernetes unless a ranked ASR forces it.
- **Numbered identifiers.** Never `BO-1`, `BR-1`, `UC-1`: concept slugs (`BO-grading-time`, `UC-EVA-submit-evaluation`); only `OI-1` is numbered.
