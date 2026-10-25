---
name: design
description: Use when designing a slice (a use case, data pipeline, or ML model) before implementing it, drafting or revising a design-of-record for an area, checking a slice against the spec and existing code for ambiguities, verifying a design is complete enough to build from, running a time-boxed experiment or spike, or adding a /design command to a repository.
---

# Design-of-Record

Work with the developer on the design-of-record for one area: how the code will realize its slices, written before the code and approved by pull request before anyone builds from it. Whatever it leaves open, the coding agent invents. It is the first step of the slice cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder); `../lifecycle/references/project-types.md` says what a data or ML design adds.

## Preconditions (refuse to start when one fails)

1. **The architecture-of-record exists** with its components and crosscutting concepts. The design takes its file paths, error shape, and conventions from there. If it is missing, stop and go to `architecture`; never assume a stack.
2. **The slice is written** with all its failure paths (use case, data contract, or model contract).
3. **The slice is settled.** If any step or failure path depends on an open `OI-*`, stop and list them. The developer decides whether to get the stakeholder's answer first or narrow what this design covers.
4. **A fresh session**, so the design reads the documents rather than an earlier conversation.

## Template

- **The project's own template wins.** If `AGENTS.md`'s Documents table names a template source for designs (a team's, a course's), use it: if it is a git clone, `git -C <clone> fetch` first (if that is not possible, say when it was last updated, `git -C <clone> log -1 --format=%cd`, and continue) and pull only on a yes; where it and this skill disagree, the template wins, and say so. Otherwise use `templates/design-of-record.md`.
- **One document per area**, at the path in the Documents table (default `docs/design/<area>.md`). If it exists, revise it in place; otherwise copy the template there with `cp -n`. Never a per-slice file.
- **Fill underneath the instructions** and leave them in place; the agent reads them on every later pass.
- **Delegate vs. keep human** (the template's "Working this document with your agent"): Claude drafts diagrams, lists files to extend, fills contract fields, proposes test rows, and checks IDs. **The developer decides which side of the pin-or-derive line each detail falls on, and every key decision; a teammate's merged review is the approval.**

## Standing rules

- **The pinning litmus.** Pin what a wrong guess would break; leave names, field types, and exact formats to the agent. In a new area, never drop the expected file paths, the contract, the decisions, or the test list.
- **No scope nobody asked for.** Every interaction or stage is labeled with the contract step it implements (`(step 7)`, `(3a)`); one with no step is a missing requirement (to the stakeholders) or scope creep (cut it).
- **New or reused.** Every component is marked new or reused, so the agent extends what exists instead of building a parallel one.
- **Code and notebooks are evidence, never authority.** For an area built before it was specified ("design-as-built"): record what the code already does, flag every departure from the confirmed spec, and let a stakeholder call each one intended, corrected, or a defect.
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 0: Challenge the slice (report only)

Read the slice contract, its rules, the requirements it depends on, the architecture's components and crosscutting concepts, and the existing code in the area. List every contradiction, ambiguous term, missing failure path, and conflict with the code or the conventions, and say where each belongs: a spec edit, an `OI-*`, or a design question. **Propose no design shape yet.** Propose the spec edits and apply them only on the developer's yes. Most findings are spec defects, not design questions.

### Step 1: The developer sketches first

Ask the developer to sketch the main flow (a sequence for a use case, a lineage for a pipeline, a train-to-serve flow for a model) and the contract rows by hand, about twenty minutes. **Wait for the sketch.** If they skip it, say what that costs: the compare step has nothing to compare, and the agent's draft becomes the anchor.

### Step 2: Claude drafts, in plan mode

In plan mode (Shift+Tab until the status bar says plan mode), propose the whole document before writing any of it, following the template: header (including the commit the code was read at), overview, components (new or reused; expected file paths for a new area), a flow diagram per slice with every step labeled, the contract with the failure response for every path, key decisions each naming the rejected alternative (the developer decides), the data model change ("No change" and why if none), the data or ML specifics that apply, reuse, the test list (every failure path plus authorization; for data, the data tests; for ML, the evaluation gate), changes to the architecture (or "None"), and open questions. **Wait for the developer to accept the shape.**

### Step 3: Compare, then write

Lay the sketch beside the draft. For each difference, the developer decides which is right and why. What the agent caught goes in; what the developer knew that the agent could not is context the spec is missing (fix it, or log an `OI-*`); additions that cite no contract step come out. Then write the design. If it changes a component, responsibility, or dependency in the architecture, change that document in the same branch and list each change.

### Step 4: The questions test (the developer runs it)

Claude cannot run this one: the session that drafted the design knows too much. Tell the developer to start a fresh session in plan mode with only the slice contract, its rules, the architecture's crosscutting concepts, and the design, and ask:

> List every point you would still have to guess to implement `<slice ID>`. For each, say what you would guess and whether a wrong guess could violate a requirement. Do not write code.

For each item they paste back, fix the design or keep the guess with one line saying why a wrong guess breaks no requirement. The design is ready when nothing on the list could break a requirement.

### Step 5: The pull request

Draft the description from the project's PR template (or `../build/templates/pull-request.md`), at most three lines per section: the slice under Traces to, the challenge findings and spec edits under What changed, the questions-test list with an answer to each item in a collapsed `<details><summary>Questions test</summary>…</details>` block under How it was verified, and under Reviewer focus the decisions most likely to be wrong and any architecture change. Push and open it only on a yes. A teammate who did not write it reviews and merges it; **no implementation starts until it is merged.** In traceability, the slice's Design cell points at the design, and Build becomes `📐 Designed` when it merges.

Hand off: after the merge, `/clear` and the `build` skill (`/implement <slice ID>`) in a fresh session.

## Experiments and spikes

When a design can't be settled without trying something (will this source join cleanly, can any model beat the baseline, how fast is this API), run a time-boxed experiment from `templates/experiment.md` (default path `docs/experiments/<slug>.md`): the question, the hypothesis and what would change your mind (written before looking), the setup to rerun it exactly, the results, and the decision with the ID of the document that now owns it. Notebook code is evidence: none of it is merged as production; the slice is designed and built through this cycle.

## The `/design` command

To give a repository this workflow as a slash command, copy the command in `templates/design-command.md` to `.claude/skills/design/SKILL.md`.
