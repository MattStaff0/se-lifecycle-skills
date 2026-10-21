# The course workflow, and which skill runs each step

<!-- Sources: the course site, https://washingtonwei.github.io/cosc-40943/ (schedule, project checkpoints,
     Method, and the modules named below; source files in Washingtonwei/cosc-40943/docs/), the
     course-templates README (tcu-cosc-40943/course-templates), and Project Pulse's docs/methodology.md
     (github.com/Washingtonwei/project-pulse, Apache-2.0). Fall 2026 dates. Where this file and the
     course site disagree, the site wins. -->

**In one sentence:** requirements and an architecture that name every part of the system at shallow depth, proved by one working vertical slice, then built out one use case at a time, with every fix looping back into the requirements and the architecture.

**How to use the skills:** each skill coaches you through one stage. It tells you what the step is for, copies the course template, drafts what the template lets the agent draft, and stops wherever the decision is yours or the client's. The orchestrators (`se-new-repo-orchestrator`, `se-slice-orchestrator`) look at your repository and tell you which stage you are in and what comes next.

## Week by week

| Week | What the course covers | What you produce | Gate | Skill |
|---|---|---|---|---|
| 1 (Aug 24–28) | SE and AI, the delegation boundary, the Napkin | Hello, Project Pulse (individual) | — | `se-new-repo-orchestrator` (the Napkin) |
| 2 (Aug 31–Sep 4) | The AI-augmented team; professionalism | Team contract signed, repo, board, Slack, first client meeting requested; charter written thin | **Checkpoint 0** (Sep 4) | `se-charter-and-governance` |
| 3 (Sep 9–11) | Requirements 1: elicitation, glossary, vision and scope | Napkin round 0 sealed in `docs/napkin-round-0.md`; interview guide; first client meeting (minutes due Sep 14); glossary, vision and scope, open issues | — | `se-new-repo-orchestrator`, `se-requirements-foundation` |
| 4 (Sep 14–18) | Requirements 2: use cases, business rules, quality attributes, SRS | Use cases, business rules, draft SRS, traceability §1–2 | TA check at week 5's studio | `se-use-cases-and-srs`, `se-requirements-foundation` (business rules) |
| 5 (Sep 21–25) | Context engineering | Specification finished; TA reviews your riskiest use case; Napkin round 1 | — | `se-use-cases-and-srs` |
| 6 (Sep 28–Oct 2) | Architecture, just enough; security as a quality attribute | Architecture-of-record (§1–5, §8, §9) merged by 11:59 pm | **Checkpoint 1** (Oct 2) | `se-architecture-of-record` |
| 7 (Oct 5–7) | Design-of-record, the design gate, the proving slice | Proving slice's area design, merged by PR, approved by a teammate (by Oct 14) | design PR merged | `se-design-of-record` |
| 8 (Oct 12–16) | Estimating in human hours; what the agent may read and run; building; code review | Build-contexts for the riskiest use cases, questions test run on them (by Oct 18); fan-out begins | — | `se-implementation-and-calibration`, `se-slice-orchestrator` |
| 9 (Oct 19–23) | Midterm (Oct 19); traceability and the release plan; testing | More use cases built and traced; traceability §3 with Release | — | `se-slice-orchestrator` |
| 10 (Oct 26–30) | Static analysis | Slice runs from tag `v0.1.0`; `.claude/settings.json`; charter must-not-do lines; calibration table; coarse estimates; Release on every row | **Checkpoint 2** (Oct 30) | `se-implementation-and-calibration`, `se-charter-and-governance` |
| 11 (Nov 2–6) | CI/CD | A pipeline with quality gates | — | — |
| 12 (Nov 9–13) | Reliability; observability | Tag `v0.2.0`; several use cases traced; CI with a gate, static analysis, observability; ratio per person and team with a forecast line; one charter rule from a review; one measured SLO; one degradation test | **Checkpoint 3** (Nov 13) | `se-slice-orchestrator` |
| 13 (Nov 16–20) | Maintainability, metrics, tech debt | The traceability honesty check | — | `se-use-cases-and-srs` (traceability) |
| 14–15 (Nov 30–Dec 9) | Ethics; MVP demos | `v1.0.0` to the client | MVP demo | — |

## The Napkin (week 1, round 0 in week 3)

Six prompts, twenty minutes, before the agent sees the problem (SE and AI, "The Napkin: six prompts"):

1. **Shape.** What kind of system this really is, in one sentence, then three to five boxes.
2. **The hard part.** The one thing that makes it not a weekend project.
3. **Bottleneck.** Where it breaks first: load, scale, or a five-person team.
4. **Stack.** What you would build it with, and one sentence of why. A boring default unless there is a reason.
5. **Kill risks.** The top three, each stated as a mechanism, not a category.
6. **Verdict.** Feasible for this team in this time, yes or no, and what you would cut first.

A seventh, **Delegate** (what goes to the agent, what stays human), waits until the first six run smoothly. **The sequence is fixed:** individually and silently (about 5 minutes), team reconcile (about 10), then the agent's napkin, then the diff (about 5). Before the agent drafts one it needs the client's real constraints (budget, incumbent systems, deadline, who will operate it), the team's actual skills, and the definition of done. Run the diff in both directions: where it named something you missed, is the mechanism real? Where you named something it missed, why? That is usually context you never gave it.

## Gates where the human decides

| Gate | Who decides | Skill that stops for it |
|---|---|---|
| Team reconciles its napkins before the agent drafts one | the team | `se-new-repo-orchestrator` |
| Meeting time, and every clause of the team contract | the team | `se-charter-and-governance` |
| Anything only the client knows: policies, numbers, priorities | the client, via the interview guide and `OPEN-ISSUES.md` | every skill |
| The use case list, reviewed before any flow is detailed; priorities | the client and the team | `se-use-cases-and-srs` |
| Every threshold in SRS §9 | the client, a measurement, or a defended team decision | `se-use-cases-and-srs` |
| The §9.1 ranking and every `KD-*` | the team | `se-architecture-of-record` |
| The proving slice (the use case exercising the top-ranked ASRs) | the team | `se-new-repo-orchestrator` |
| The sketch before the agent drafts; accepting the design's shape; every key decision | the developer | `se-design-of-record` |
| The design approved: a teammate who did not write it merges the PR | a teammate | `se-design-of-record` |
| Every hour in every estimate | the team (coarse), the owner (refined) | `se-implementation-and-calibration` |
| The trunk pause: can you say what changed and why without asking the agent? | the developer | `se-implementation-and-calibration` |
| Every new dependency | a person | `se-implementation-and-calibration` |
| Approving and merging the implementation PR | the reviewer | — |
| The retrospective's answers; which rows leave `v1.0.0` | the owner; the client | `se-implementation-and-calibration` |

## Phase C: one use case at a time

Design, implementation, and any agent review each start in a **fresh session** that reads only the merged documents.

1. **Design** (`/design`): challenge loop → sketch → agent draft in plan mode → compare → questions test → PR merged by a teammate. No implementation branch until it merges.
2. **Build-context and refined estimate:** the issue cites the use case and the design and stops there; run the questions test on it.
3. **Implement** (`/implement`): trunk commit, **pause**, a security rule or migration alone, one commit per leaf, each with its tests.
4. **Review** against the contract: the triage line, the six-item checklist, labeled comments.
5. **Close** (`/close-use-case`): honest traceability, hours since each estimate ÷ its midpoint, the five-minute retrospective into the calibration table, a charter rule for anything corrected twice, the release plan revised. Then `/clear`.

## What a change reopens

| The change | What it reopens |
|---|---|
| Stays inside one use case area and adds no architecturally significant requirement | A spec edit, then that area's design-of-record, through its gate |
| Adds a use case area, component, external integration, or crosscutting subsystem, or adds or re-ranks an ASR | The architecture-of-record **first** |
| Overturns a key decision | A superseding `KD-*`, before any area designs against it |
| Changes scope | Vision and scope first; each new use case enters the release plan with a Release value |

## Starting from an existing codebase

**The code is evidence, never authority.** A spec recovered from code describes what the code does, defects included.

1. Recover in the usual order: glossary, use cases, business rules, then the architecture as built. Mark every drafted statement as recovered.
2. Whoever owns the intent (the client, never the team alone) calls each statement intended, corrected, or a defect. A defect becomes an `OI-*` or an issue, not a requirement.
3. A design-of-record only where work lands, written as built, flagging every departure from the confirmed spec.
4. Traceability starts honest: a recovered use case is `✅ Built` / `🔎 None` until a test exercises it.

## Scaling to the size of the change

| Change | Keep |
|---|---|
| A bug fix | No `/design`. If a use case covers the behavior, fix the code, add a regression test, and cite the use case. If none does, add the extension first. |
| A small change inside one area | Amend the use case and add a row to its design's test list. |
| A small project (one or two people, short-lived) | A floor of three: the agent charter (opening with a short vision and scope), use cases with their extensions and inline business rules, and a test list per use case. |
| A team, real stakeholders, a long-lived system | The full method. |
