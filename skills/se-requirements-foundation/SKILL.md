---
name: se-requirements-foundation
description: Use when preparing for or debriefing a client interview, defining project vocabulary or a glossary, writing business objectives, success metrics, or MVP scope, cataloging business rules, or when a business question comes up that only the client can answer.
---

# SE Requirements Foundation

Work with the developer on the client-facing requirements documents: the client interview guide (one copy per meeting), the project glossary, vision and scope, open issues, and the business rules. In the course: interview guide before the first client meeting; glossary, vision and scope, and open issues in **week 3**; business rules in **week 4**, alongside use cases and the SRS (`se-use-cases-and-srs`). All are reviewed at **Checkpoint 1**.

## Course templates

The course templates (`tcu-cosc-40943/course-templates`) are the authority. This skill adds the workflow around them and never restates a template's sections; where this skill and a template disagree, the template wins, and say so.

1. **Find the current copy.** Local clone: `$COURSE_TEMPLATES`, else `~/Desktop/tcu-fall-2026/software-engineering/course-templates`. Run `git -C <clone> fetch` (if fetching is not possible, say when the clone was last updated, `git -C <clone> log -1 --format=%cd`, and continue); if the clone is behind, show the developer `git -C <clone> log --oneline HEAD..@{u}` and pull only on a yes. With no clone: `gh api -H "Accept: application/vnd.github.raw" repos/tcu-cosc-40943/course-templates/contents/<path>`.
2. **Copy before writing, never over.** Before drafting, copy the templates this step needs into the target repo, on a branch, with no-clobber (`cp -n`). Report what was copied and what already existed. Never draft a course document from scratch, and never overwrite a filled one.
3. **Fill underneath the instructions.** Each section opens with italic `[bracketed]` instructions: follow them, run the section's checklist, write underneath, and leave the instructions in place until the document is finished. Worked examples taken from another project (Project Pulse, the Cafeteria Ordering System) come out when you write your first real entry in that section, because their identifiers would pollute the ID checks; the instructions stay.
4. **Delegate vs. keep human.** Where the template has a "Working … with your agent" section, draft only what its Delegate list allows; for each Keep-human item, explain the choice to the developer and wait for their answer.

| Course template | Target | When |
|---|---|---|
| `requirements/client-interview-guide.md` | `docs/requirements/client-interview-YYYY-MM-DD.md`, one copy per meeting | before each client meeting |
| `requirements/project-glossary.md` | `docs/requirements/project-glossary.md` | week 3 |
| `requirements/vision-and-scope.md` | `docs/requirements/vision-and-scope.md` | week 3 |
| `requirements/OPEN-ISSUES.md` | `docs/requirements/OPEN-ISSUES.md` | week 3, before anything is routed to it |
| `requirements/business-rules.md` | `docs/requirements/business-rules.md` | week 4 |

## Standing rules

- **You cannot answer for the client.** When a policy, priority, number, or rule is something only the client knows, never pick a plausible option. Before a meeting, the question goes into this meeting's interview guide; otherwise it becomes an `OI-*` in `OPEN-ISSUES.md`, phrased so it has a definite answer, with who can answer it.
- **No invented facts.** Every number, baseline, target, pain point, volume, and rule cites the brief, the meeting notes, or the client. A plausible figure nobody stated ("from 2 hours to 1") is an `OI-*`, not a fact. A team judgment the template asks for (a risk's probability and impact) is labeled as the team's estimate and comes from the developer, never the agent.
- **One word, one concept.** The agent searches the repo with the words it is given; a synonym is a search that finds nothing. The glossary fixes one term per concept, used in code identifiers and UI text.
- **Name-based slugs.** `BO-<slug>`, `SM-<slug>`, `FEAT-<slug>`, `RI-<slug>`, `AS-<slug>`, `BR-<slug>`: never numbered, never renumbered. The one exception is `OI-1` upward.
- **Enthusiasm is not commitment.** Ask for the artifact they use today ("show me the spreadsheet"), and ask the forcing question for scope: if we can deliver only one of these, which one?
- **Coach, don't just produce.** Before each step, tell the developer in one or two sentences what it is for and what they decide in it. Interview them a few questions at a time, with multiple-choice answers where that helps; "ask the client" is always an allowed answer.
- **Git:** work on a branch; propose commits and the pull request, and make them only on a yes.

## Workflow

### Step 1: Before a client meeting

Copy the interview guide for this meeting. Then run it as its own instructions say: give the agent the brief and the guide **including its instructions**; rewrite each question type in the client's domain and list the acronyms to define; interview the developer on the brief ("a few questions at a time, with multiple-choice answers"), and add every *ask the client* to this guide under the section it belongs to (the rewritten questions and the acronym list go at the top of this meeting's copy). The developer picks which questions are worth the client's hour, sorted by what it costs to stay wrong. Remind them of the guide's two rules: **listen before you build**, and **commit to nothing** ("Let me write that down and bring it back to the team"). Then the guide's "Before you go" checklist: three roles, the shortlist sent the day before.

### Step 2: After the meeting (within 24 hours)

File each section of the meeting record where the guide's **After the meeting** table says it feeds, in the client's words, and copy every unanswered question into `OPEN-ISSUES.md` as an `OI-*`. Remind the developer to send the client the notes and open questions, and to commit the meeting file the same day.

### Step 3: The glossary

Follow the template: only the entries that earn their place (terms two stakeholders use differently, generic-sounding terms, the client's acronyms, terms the team invented); one `###` heading per term, alphabetical; a one-to-three-sentence definition of the concept, not the implementation; **Synonyms**, **Not to be confused with**, and **Source** lines where they help. The synonym lines tell the agent which words never to use for a class or table.

### Step 4: Vision and scope

Work section by section, in the template's order, following each instruction and checklist; the brief, meeting notes, and `OPEN-ISSUES.md` are the only sources. Every objective (`BO-<slug>`) carries a number and the success metrics that measure it; each metric (`SM-<slug>`) carries its indicator, source, baseline, and target with a deadline, and every metric traces to an objective. A target the client has not given is an `OI-*`. Features (`FEAT-<slug>`, §4.2) are capabilities in stakeholder terms, never "create, edit, delete X" (that is a use case list) and never one-to-one with a single use case area. The MVP scope (§4.3) is decided with the client, not by the agent.

### Step 5: Business rules (week 4)

Copy the template and follow its format: rules grouped under **topic** headings in §2, each a bold `BR-<slug>`, the rule in one sentence about the business in the client's words, then its **Source** (a document, or the person and date). The five shapes (fact, constraint, action enabler, inference, computation) are for recognizing a rule in a meeting, never a field in the document. A rule whose source is the team is flagged as a candidate for the specification instead, as the template's third worked example shows. Never let the agent invent a threshold or formula; ask for the client's spreadsheet.

### Step 6: Keep `OPEN-ISSUES.md` a queue

New questions go in the **Open** table with why it matters, who can answer, and the date raised. Before a meeting, pick the shortlist by what it costs to stay wrong (the IDs keep the order they were raised in) and send it to the client the day before. When one is answered, move it to the **Resolved** table (answer, who answered, date, and where it was filed), and put the substance in its owning document. Never renumber.

## Gotchas

- **Features vs. use cases.** A feature that reads like one interaction or a CRUD list is a use case area mis-cast as a feature; pull it up to the capability theme it belongs to.
- **Invented baselines.** The agent will write a confident before-and-after number nobody stated. Flag every unsourced number and replace it with an `OI-*` until the client confirms it.
- **A solution is not a requirement.** "Then I pick the state from a drop-down" goes under the guide's Initial ideas; ask why until you reach the need underneath, and write the need.
