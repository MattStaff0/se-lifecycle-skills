---
name: discovery
description: Use when preparing for or debriefing a stakeholder interview or client meeting, defining project vocabulary or metric definitions, writing business objectives, success metrics, or MVP scope, cataloging business rules or formulas, or when a question comes up that only a stakeholder can answer.
---

# Discovery

Work with the developer on the stakeholder-facing documents: an interview record per conversation, the glossary, vision and scope, open issues, and the business rules. It is step 2 of the cycle (`../lifecycle/references/cycle.md`, relative to this skill's folder), and it is where every "only a stakeholder knows" question comes back to. For how discovery differs for data and ML projects, read `../lifecycle/references/project-types.md`.

## Templates

1. **The project's own template wins.** If `AGENTS.md`'s Documents table names a template source for a document (a team's template folder, or another repository such as a course's), use it: if it is a git clone, `git -C <clone> fetch` first (if that is not possible, say when it was last updated, `git -C <clone> log -1 --format=%cd`, and continue) and pull only on a yes. Where that template and this skill disagree, the template wins, and say so.
2. **Otherwise use the bundled template** in this skill's `templates/` folder.
3. **Copy before writing, never over.** Copy to the path in the Documents table (defaults below) with `cp -n`, on a branch, and say what was copied and what already existed. Never draft one of these documents from scratch, and never overwrite a filled one.
4. **Fill underneath the instructions.** Follow each section's italic instruction and run its checklist; leave the instructions in place until the document is stable. Example entries come out when you write the first real one.
5. **Delegate vs. keep human.** Draft only what the template's "Working this document with your agent" Delegate list allows; for each Keep-human item, explain the choice to the developer and wait.

| Template | Default path |
|---|---|
| `templates/interview-guide.md` | `docs/requirements/interview-YYYY-MM-DD.md`, one per conversation |
| `templates/glossary.md` | `docs/requirements/glossary.md` |
| `templates/vision-and-scope.md` | `docs/requirements/vision-and-scope.md` |
| `templates/OPEN-ISSUES.md` | `docs/requirements/OPEN-ISSUES.md` (copy it before anything is routed to it) |
| `templates/rules.md` | `docs/requirements/rules.md` |

## Standing rules

- **You cannot answer for a stakeholder.** When a policy, priority, number, definition, or rule is something only a stakeholder knows, never pick a plausible option. Before a conversation, the question goes into that conversation's interview file; otherwise it becomes an `OI-*`, phrased so it has a definite answer, with who can answer it.
- **No invented facts.** Every number, baseline, target, pain point, volume, metric definition, and rule cites the brief, an interview, a document, or a measurement; anything else is an `OI-*`. A judgment the template asks the team for (a risk's likelihood and impact) is labeled as the team's estimate and comes from the developer, never the agent.
- **One word, one concept.** The agent searches the repo with the words it is given; a synonym is a search that finds nothing. The glossary fixes one term per concept, used in code, column names, and UI text.
- **Name-based slugs.** `BO-`, `SM-`, `FEAT-`, `RI-`, `AS-`, `DE-`, `BR-` are slugs, never numbered; only `OI-1` upward is a number.
- **Enthusiasm is not commitment.** Ask for the artifact they use today ("can I see the spreadsheet?"), and the forcing question for scope: if we can deliver only one of these first, which one?
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it. Interview them a few questions at a time, with multiple-choice answers where that helps; "ask the stakeholder" is always an allowed answer.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 1: Before a conversation

Copy the interview guide for this conversation and run its "Before you go" list with the developer: interview them on the brief first (a few questions at a time; every "ask the stakeholder" goes into the guide under the section it belongs to), rewrite the generic questions in the stakeholder's terms, list the acronyms to define, and let the developer pick the shortlist by what it costs to stay wrong. For data projects include section 9; for ML, section 10. Remind them of the two rules for the room: **listen before you build**, and **commit to nothing**.

### Step 2: After the conversation (within 24 hours)

File each section where the guide's "After the conversation" table says it goes, in the stakeholder's words, and copy every unanswered question into the open-issues file as an `OI-*`. Remind the developer to send the stakeholder the notes and the open questions, and to commit the interview file the same day.

### Step 3: The glossary

Only the entries that earn their place: terms two stakeholders use differently, generic-sounding terms, metric definitions, acronyms, invented terms. One `###` per term, alphabetical, a concept definition, with Synonyms, Not to be confused with, and Source lines. When two stakeholders define a term differently, that is an `OI-*`, not a choice.

### Step 4: Vision and scope

Section by section, in the template's order, from the brief, interview records, and open issues only. Every objective carries a number and its metrics; every metric its indicator, source, baseline, and target with a date; a missing baseline or target is an `OI-*`. Features are capabilities, never CRUD lists or one-to-one with an area. The stakeholder decides the MVP line; the developer reads the vision statement aloud to them.

### Step 5: Business rules

Rules under topic headings, each a bold `BR-<slug>`, one sentence about the business, then its source. The five shapes are for recognizing a rule, never a field. For data and ML projects, metric definitions and formulas are rules too: reproduce them exactly from the stakeholder's artifact. A rule whose only source is the team is flagged as a candidate for the requirements.

### Step 6: Keep open issues a queue

New questions go in Open with why it matters, who can answer, and the date. Before each conversation, pick the shortlist by cost of staying wrong (the IDs keep the order they were raised in) and send it the day before. When one is answered, move it to Resolved (answer, who, date, where it was filed) and put the substance in the owning document. Never renumber.

## Gotchas

- **Features vs. slices.** A feature that reads like one interaction or a CRUD list is a slice area mis-cast as a feature; pull it up to the capability it serves.
- **Invented baselines and definitions.** The agent will write a confident before-and-after number, or a reasonable-sounding definition of "active user", that nobody stated. Flag every unsourced one as an `OI-*`.
- **A solution is not a requirement.** "Then I pick the region from a drop-down" goes under the interview's Initial ideas; ask why until you reach the need, and write the need.
