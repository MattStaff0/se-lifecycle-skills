<!-- Source: the course's Team Contract page, https://washingtonwei.github.io/cosc-40943/team-contract/
     (source file: Washingtonwei/cosc-40943, docs/team-contract.md). Copied verbatim below the line.
     If that page has changed, the page wins: fetch it with
     gh api -H "Accept: application/vnd.github.raw" repos/Washingtonwei/cosc-40943/contents/docs/team-contract.md

     How the course says to fill it in: ten minutes, together, out loud. Fix the meeting time first.
     Write what you will actually do ("Slack, replies within 24 hours on weekdays", not "we will
     communicate effectively"). Each member signs in their own commit. Checkpoint 0 checks: a named day
     and time in clause 1, one signature per member each in its own commit, a clause 7 that says what
     actually happens, and the file at docs/team-contract.md with the header naming the repo owner.
     For clause 7, the Professionalism module's four-step protocol (Observe, Analyze, Investigate, Fix)
     is the course's shape for raising it. -->

# Team Contract: <team name>

**Project:** <client project name>
**Members:** <every member's name>
**Repository:** <url>, owned by <name>
**Signed:** <date>

## 1. Meeting time

We meet every **<day>** at **<time>** in **<place or link>**, for <duration>.
A member who cannot attend tells the team **<how far ahead>** and reads the minutes.

## 2. Communication

Primary channel: **<Slack channel>**. Client contact goes through **<name>**.
We reply within **<n>** hours on weekdays. Anything urgent: **<how>**.

## 3. How we decide

Routine calls: **<e.g. whoever owns the use case decides>**.
Anything affecting the whole team: **<e.g. discussed at the weekly meeting, majority, ties go to the project lead>**.
A decision that survives the meeting is written down in **<where>**.

## 4. How work is claimed

Work is divided **by use case, not by layer**. One member owns a use case end to
end: front end, back end, tests, and the pipeline.
Claiming: **<e.g. assign yourself the sub-issue and move the card>**.
Nobody is the "front-end person" or the "tester".

## 5. Git workflow and review

Coding conventions (naming, formatting, layout) live in `AGENTS.md`, not here.
This clause is about how work moves.

Branch per sub-issue, named **<convention, e.g. feat/42-short-slug>**.
Never push to `main`. Every change arrives as a pull request.
A pull request needs **<n>** approving review(s) from someone who does not own the use case.
A reviewer reads the issue before the diff. Blocking a merge: **<what blocks it>**.

## 6. Working with AI

We use **<tools>**. Our charter lives in `AGENTS.md`.
Every member can explain any line submitted under their name.
We do not merge agent output that nobody has read.
Additional limits we agree on: **<anything else>**.

## 7. When someone does not deliver

First: **<who raises it, and how soon>**. We attack the problem, not the person.
If it happens again: **<what the team does>**.
Still unresolved: we escalate to our TA, then to the instructor. We escalate early.

## Signatures

Each member adds their own line, in their own commit.

- <name>, <date>
