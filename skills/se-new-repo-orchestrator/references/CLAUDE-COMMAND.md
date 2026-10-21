# Slash command: `/new-repo-setup`

Copy the block below into the target repository as `.claude/skills/new-repo-setup/SKILL.md` (or `.claude/commands/new-repo-setup.md`, the older form).

**Source:** no Project Pulse equivalent. Built from the course-templates README's copy instructions (`tcu-cosc-40943/course-templates`) and the se-new-repo-orchestrator skill. The copy step never overwrites: the README warns that a team's `traceability.md` already holds its first tables by week 8.

````markdown
---
description: "Set up docs/ from the course templates without overwriting anything, then point to the next lifecycle stage."
argument-hint: "(none)"
disable-model-invocation: true
---

# /new-repo-setup: course templates into docs/, safely

## Preconditions

- The current directory is the team repository's root, on a branch other than `main`. If on `main`, propose a branch name and switch only on a yes.

## Phase 1: Find the templates (read only)

`T=${COURSE_TEMPLATES:-../course-templates}`. If `$T` does not exist, say so and offer to clone `https://github.com/tcu-cosc-40943/course-templates.git` next to the project; clone only on a yes. If it exists, run `git -C "$T" fetch`; if it is behind, show `git -C "$T" log --oneline HEAD..@{u}` and pull only on a yes.

## Phase 2: Plan the copy (read only)

List what would be copied and what already exists, file by file:

- `$T/requirements/*` → `docs/requirements/` (all seven, including `client-interview-guide.md`; the README says to copy them all now)
- `$T/traceability.md` → `docs/traceability.md`
- `$T/design/architectural-design.md` → `docs/design/architectural-design.md`
- `$T/.github/PULL_REQUEST_TEMPLATE.md` → `.github/PULL_REQUEST_TEMPLATE.md`

`design/design-of-record.md` is deliberately not copied: it is copied once per use case area, as `docs/design/<area>.md`, when that area is designed. **Never overwrite an existing file.** If a template has gained a section the team's copy lacks (traceability §3 in week 8), say so; the README's rule is to paste that section into the existing file, not copy the file again.

**Stop. Copy only on the developer's yes.**

## Phase 3: Copy, then hand off

```bash
mkdir -p docs/requirements docs/design .github
cp -n "$T"/requirements/*.md docs/requirements/
cp -n "$T"/traceability.md docs/
cp -n "$T"/design/architectural-design.md docs/design/
cp -n "$T"/.github/PULL_REQUEST_TEMPLATE.md .github/
```

Report what was copied and what was skipped. Propose the commit and a pull request, and make them only on a yes. Then name the next stage from the course lifecycle and its skill (se-new-repo-orchestrator diagnoses it). Leave every template's italic `[bracketed]` instructions in place.
````
