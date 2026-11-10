---
name: lifecycle
description: Use when starting a new project or repository, when unsure which spec-driven step or document comes next, before prompting an agent to plan or build a new project (the Napkin), when setting up docs/ from templates, when picking a proving slice, or when auditing a project's documents against the cycle.
---

# Lifecycle

Find out where a project is in the spec-driven cycle, say what comes next and why, and hand off to the skill for that step. Works for application, data, and ML projects, and for a mix. Never let the agent invent requirements, architecture, or answers a stakeholder hasn't given. Read `references/cycle.md` (the cycle, its gates, and how to size it to the work) first; `references/project-types.md` says how each step changes by project type.

## Standing rules

- **The repository is the only memory.** A decision not committed to the spec, the architecture, or `AGENTS.md` does not exist for the agent.
- **Cite, never copy.** Slices, rules, and decisions are cited by slug ID; only `OI-1` upward is numbered.
- **Never answer for a stakeholder.** A missing policy, number, or definition becomes an `OI-*` in the open-issues file.
- **Where a skill stops is its most important line.** Stop at every gate in the cycle's table.
- **Coach.** Say in one or two sentences what each step is for and what the developer decides in it; the developer is learning the method, not just filling documents.
- **Git:** work on a branch; propose commits and pull requests, and make them only on a yes.

## Step 1: Diagnose (read only)

Read `AGENTS.md` first: its project types, its Documents table (where each document lives and which template it uses), and any `**Profile:**` line (if one names a profile in `references/profiles/`, read it: it can change templates, the calendar, and a few rules). Then check what exists and how far each is filled beneath its template's instructions: the charter and `.claude/settings.json`; the glossary, vision and scope, open issues, rules; the slice contracts for each project type; requirements; traceability (§1, §2, §3); the architecture-of-record (which sections); designs; experiments; the gate (analyzers in the build, a pull request workflow, checks required on the main branch); tags and releases. Report:

1. **The step the project has reached** in the cycle.
2. **What the next step needs**, and any deadline the profile sets.
3. **The gap**, as a short ordered list, each item naming the skill that handles it.

If there is no `AGENTS.md`, the first item is the charter.

**STOP.** Wait for the developer to confirm and pick what to work on. When a step's skill finishes, come back and rerun this step, so the next step is named from the repository's state, not from memory.

## Step 2: Run the chosen step

| Step | Hand off to |
|---|---|
| The Napkin | here: below |
| Charter, deny rules, team contract | `charter` |
| Set up `docs/` | here: below |
| Interviews, glossary, vision and scope, open issues, rules | `discovery` |
| Slice list, slice contracts, requirements, traceability | `specification` |
| Architecture-of-record | `architecture` |
| Pick the proving slice | here: below |
| Static analysis and the pull request gate | `static-analysis` |
| Design a slice; run an experiment | `design` |
| Estimate, build, review, close | `build` (one slice at a time: `slice`) |

### The Napkin

Use the six prompts and the fixed sequence in `references/cycle.md`. **Ask each person to write their own napkin silently first, then reconcile as a team. Stop until they say they have.** Only then ask for the real constraints (budget, existing systems, deadline, who operates it, the team's skills, the definition of done), draft the agent's napkin, and run the diff with them in both directions. Reject any kill risk stated as a category ("integration issues", "data quality") and ask for the mechanism ("the vendor exports CSV nightly, so the real-time dashboard is dead on arrival"; "only 300 labeled examples exist, so a supervised model can't beat the current rule"). Save it as `docs/napkin.md` (one heading per prompt: the team's answer, the agent's, what the diff found).

### Setting up docs/

For each document the project's types need, copy its template to the path in the Documents table (the profile's or team's template source if one is named, otherwise the bundled template in the owning skill's `templates/` folder), with `cp -n`, on a branch. List what will be copied and what already exists, **copy only on a yes**, and never overwrite. Report what was copied and skipped, and propose the commit.

### Picking the proving slice

The proving slice is the one that exercises the highest-ranked architecturally significant requirements, crosses the trust boundary, or depends on the riskiest external system or data source; for ML, often the model whose baseline is hardest to beat. Never a login screen or a simple list. Propose candidates with the risks each one exercises; **the team picks**. Then hand off to `design`, and to `static-analysis` so the gate is on before the first feature merges.

## Gotchas

- **Prompting before thinking.** Never skip the silent human step in the Napkin or the design sketch; the agent's answer seen first anchors everyone on it.
- **Infrastructure by feature list.** No microservices, queues, Spark, or Kubernetes unless a ranked requirement forces it.
- **Numbered identifiers.** Never `BO-1`, `BR-1`, `UC-1`: concept slugs. Only `OI-1` is numbered.
- **A notebook is not a slice.** Exploratory results become an experiment record and a decision; production code is designed and built through the cycle.
