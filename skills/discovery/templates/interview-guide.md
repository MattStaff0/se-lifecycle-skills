# Stakeholder Interview: _[YYYY-MM-DD]_

**Project:** _[name]_ · **Stakeholder(s):** _[name, role, for everyone in the room, including whoever only listened]_ · **Team:** _[lead, scribe, observer]_ · **Recording / photos:** _[asked? granted?]_

_[How to use this file. Copy it once per conversation as `interview-YYYY-MM-DD.md` and commit it the same day. Going in, it is your script; coming out, it is the record. Write answers underneath each question in the stakeholder's words, not yours, especially the nouns: they become the glossary. Leave these instructions in place; delete the example questions you rewrote.]_

## Before you go

- [ ] **The agent interviewed you first.** Give it the brief and this file, and ask it to interview you a few questions at a time with multiple-choice answers. Everything the brief already answers is not worth a stakeholder's minute. Every "ask the stakeholder" goes into the section below where it belongs.
- [ ] **Questions rewritten for this domain.** The prompts below are generic. Rewrite each in the stakeholder's terms, and list the acronyms you need defined.
- [ ] **A shortlist, sorted by what it costs to stay wrong.** You won't get through all of it. A question whose wrong guess sends the team the wrong way for a month goes first.
- [ ] **Shortlist sent the day before**, so they arrive with answers instead of promises.
- [ ] **Roles:** one lead asks; one scribe writes and does not ask; one observer watches what is not said.

**Two rules for the room.** *Listen before you build:* an idea proposed in the first twenty minutes ends the discovery, because they start reacting to it instead of describing their world. *Commit to nothing:* "let me write that down and bring it back to the team".

## Our questions

_[The rewritten questions and the acronym list, in the order you will ask them.]_

## 1. Their world (5 min)

_[Their role, their team, what a normal week looks like. What words do they use that an outsider would get wrong?]_

## 2. The goal, and why now (10 min)

_[What has to be true when this is done, and how they would measure it. What happens if nothing changes. Who asked for it.]_

## 3. How it works today (15 min)

_[Walk through the process step by step, as it happens, not as it should. Ask to see the spreadsheet, form, report, or screen they use; ask to photograph it.]_

## 4. What is hard about it (10 min)

_[Where time goes, where mistakes happen, what they work around. Ask for numbers: how often, how long, how many.]_

## 5. What already works (5 min)

_[What must not get worse. Anything they would refuse to give up.]_

## 6. Volumes and scale (5 min)

_[How many users, records, requests, rows, files; how fast it grows; the busiest time.]_

## 7. Who uses it (5 min)

_[Each kind of user, what they need, how comfortable they are with tools.]_

## 8. Rules and constraints (10 min)

_[Listen for "only … may …", "if … then …", "must be calculated as …", "unless …". Ask where each rule is written down. Budget, deadline, systems it must work with, regulations, data that is personal or regulated.]_

## 9. Data (data and ML projects, 10 min)

_[Every source, who owns it, how it arrives and how often. How each key metric is defined, in their words ("what counts as an active customer?"). How fresh it must be. What happens today when data is late or wrong.]_

## 10. The decision (ML projects, 10 min)

_[What decision the model supports and who makes it today. What it is compared with now (a rule, a person). What a false positive costs versus a false negative. Which labels exist and how trustworthy they are. Limits on latency, cost, explainability, fairness.]_

## 11. Systems it touches (5 min)

_[Every system it reads from or writes to, who owns each, and how you would get access.]_

## 12. After hand-off (5 min)

_[Who runs and maintains it once the team is gone. What skills and budget they have.]_

## 13. Other stakeholders (5 min)

_[Who else cares, who could block it, who should be in the next conversation.]_

## 14. Read-back (must happen, 5 min)

_[Summarize the process and the goal back to them in their words, and let them correct you. Only now, if they want ideas, share yours.]_

## 15. Close (must happen, 5 min)

_[What happens next, when you will follow up, and who answers the questions you couldn't settle.]_

## After the conversation (within 24 hours)

| Section | Files into |
|---|---|
| 1, 2 | Glossary; vision and scope (background, objectives) |
| 3, 4, 5 | Vision and scope (how it works today, pain points); slice candidates |
| 6, 7, 12, 13 | Vision and scope (stakeholders, operation); requirements (scale, constraints) |
| 8 | Business rules; requirements (constraints) |
| 9 | Glossary (metric definitions); business rules (computations); data contracts |
| 10 | Model contracts; requirements (model quality) |
| 11 | Requirements (external interfaces); architecture context |
| 14 | Vision and scope (vision statement, scope) |
| Anything unanswered | Open issues, as an `OI-*` with who can answer |

**Initial ideas:** _[Solutions anyone floated, here and nowhere else yet. A solution the stakeholder already picked is not a requirement; ask why until you reach the need, and write the need.]_

**Disagreements and hesitations:** _[The observer's notes: two people using one word differently, a question answered by the wrong person, a pause before "yes".]_

Send the stakeholder your notes and the open questions within 24 hours, then commit this file.
