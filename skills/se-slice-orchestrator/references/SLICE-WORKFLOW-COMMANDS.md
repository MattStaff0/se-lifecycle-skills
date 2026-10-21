# Slice workflow commands

Paths below are relative to this skill's folder (`se-slice-orchestrator/`), so `../` is the folder holding all eight skills. Each command lives in one place, so it cannot drift into two versions. Install each as `.claude/skills/<name>/SKILL.md` in the target repository (the older `.claude/commands/<name>.md` also works), committed by pull request.

| Slice stage | Command | Where it is |
|---|---|---|
| 2. Design-of-record | `/design <UC-ID>` | `../se-design-of-record/templates/design-command.md` |
| 3. Build-context and refined estimate | `/estimate <UC-ID> refined`, and the issue template | `../se-implementation-and-calibration/templates/claude-commands.md`; `../se-implementation-and-calibration/templates/build-context-issue.md` (install as `.github/ISSUE_TEMPLATE/use-case.md`) |
| 4. Trunk and leaves | `/implement <UC-ID>` | `../se-implementation-and-calibration/templates/claude-commands.md` |
| 5. Review and close | the review checklist, then `/close-use-case <UC-ID>` | `../se-implementation-and-calibration/templates/pr-and-code-review-checklist.md`; `../se-implementation-and-calibration/templates/claude-commands.md` |
