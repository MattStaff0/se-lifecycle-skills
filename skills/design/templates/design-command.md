# Slash command: `/design`

Copy the block below into a repository as `.claude/skills/design/SKILL.md` (or `.claude/commands/design.md`), committed by pull request, so everyone on the team runs the same workflow. With the `sdd` plugin installed you can also just use `/sdd:design`.

**Source:** adapted from Project Pulse's `.claude/commands/design.md` (github.com/Washingtonwei/project-pulse, Apache-2.0). Changes: generalized from one Spring and Vue codebase to any project type (use case, data contract, or model contract), with the order sketch → plan-mode draft → compare → questions test → pull request, and the paths read from the project's `AGENTS.md`.

````markdown
---
description: "Design a slice before code: challenge it, take the developer's sketch, propose the design in plan mode, compare, write the area's design-of-record. Stops before any code."
argument-hint: "<slice ID>"
disable-model-invocation: true
---

# /design: turn a slice into a design-of-record

Take the slice named in `$ARGUMENTS` (a `UC-`, `DP-`, or `MD-` ID) and produce its design-of-record for its area, at the path `AGENTS.md`'s Documents table gives (default `docs/design/<area>.md`, lowercase area code). Design is its own reviewed stage and **stops before any code**; `/implement <ID>` builds from it after it merges.

## Preconditions

- `$ARGUMENTS` names a slice written with all its failure paths. If not, ask which one.
- The architecture-of-record exists with its components and crosscutting concepts. If not, stop: architecture first.
- No step of the slice depends on an open `OI-*`. If one does, stop and list them; the developer decides whether to ask the stakeholder first.
- A fresh session. If this one carries unrelated work, ask for `/clear`.

## Standing rules

- **The spec is authoritative but not infallible: challenge it.** Ambiguous step, contradiction, a stale constraint: ask or push back; never silently comply or silently invent. When in doubt, or when you see a better way, shout.
- **Never answer for a stakeholder.** A policy question becomes an `OI-*`.
- **Cite, never copy.** The design cites slices, rules, and requirements by ID and says only how.
- **Pin what a wrong guess would break**; leave the rest to `/implement`.
- **Every interaction or stage names the contract step it implements.** One that names none is a missing requirement or scope creep.
- **The developer decides** each key decision and which side of the pin-or-derive line a detail falls on.
- Propose commits and the pull request; make them only on a yes.

## Phase 1: Challenge the slice (read only, report only)

Read the slice contract (search for its ID; don't read the file whole), its rules, the requirements it depends on, its glossary terms, the architecture's components and crosscutting concepts in full, any existing design for the area (revise in place, never a per-slice file), and the existing code. Code that exists but was never designed: the code is evidence, never authority; flag every departure from the spec.

Report every contradiction, ambiguous term, missing failure path, and conflict with the code or the conventions, and where each belongs: a spec edit, an `OI-*`, or a design question. Propose spec edits and apply them only on a yes. **Propose no design shape in this phase.**

## Phase 2: The developer's sketch

Ask the developer to sketch the main flow (sequence, lineage, or pipeline) and the contract rows by hand, about twenty minutes. **Stop until they share it,** or say skip (and say what skipping costs: nothing to compare against).

## Phase 3: Propose the design (plan mode)

If not already in plan mode, ask the developer to enter it. Propose the whole document from the project's design template: header, overview, components (new or reused; expected file paths for a new area), one flow diagram per slice with every step labeled, the contract (with the failure response for every path), key decisions with the rejected alternative, the data model change (or "No change" and why), the data or ML specifics that apply, reuse, the test list (every failure path plus authorization), changes to the architecture (or "None"), and open questions. **Edit nothing. Stop until the developer accepts the shape.**

## Phase 4: Compare, then write

For each difference from the sketch, the developer decides which is right. What the agent caught goes in; what the developer knew that the agent couldn't becomes a spec fix or an `OI-*`; additions with no contract step come out. Then write the design (copy the template first if the file doesn't exist), leaving its instructions in place, and apply any approved change to the architecture in the same branch.

## Phase 5: Questions test and pull request

Stop and tell the developer to run the questions test in a **fresh** session in plan mode, with only the slice contract, its rules, the architecture's crosscutting concepts, and this design:

> List every point you would still have to guess to implement `<slice ID>`. For each, say what you would guess and whether a wrong guess could violate a requirement. Do not write code.

For each item they paste back: fix the design, or keep the guess with a one-line reason. Then draft the pull request description (the project's PR template if it has one; questions-test list in a collapsed `<details>` block; Not verified filled). Push and open it only on a yes. A teammate who didn't write it reviews and merges it. In traceability, set the slice's Design cell; Build reads `📐 Designed` once it merges.

Hand off: after the merge, `/clear`, then `/implement <slice ID>`.
````
