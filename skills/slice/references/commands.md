# Slice commands

With the `sdd` plugin installed, the skills are available everywhere as `/sdd:<skill>`. To give a repository the same workflow for everyone on the team, without the plugin, copy these commands into it as `.claude/skills/<name>/SKILL.md`, committed by pull request. Paths below are relative to this skill's folder (`slice/`), so `../` is the folder holding all eight skills.

| Slice stage | Command | Where it is |
|---|---|---|
| 2. Design | `/design <ID>` | `../design/templates/design-command.md` |
| 3. Build-context and refined estimate | `/estimate <ID> refined`, and the issue template | `../build/templates/commands.md`; `../build/templates/build-context-issue.md` (install as `.github/ISSUE_TEMPLATE/slice.md`) |
| 4. Trunk and leaves | `/implement <ID>` | `../build/templates/commands.md` |
| 5. Review and close | the review checklist, the PR template, then `/close <ID>` | `../build/templates/review-checklist.md`; `../build/templates/pull-request.md` (install as `.github/pull_request_template.md`); `../build/templates/commands.md` |
