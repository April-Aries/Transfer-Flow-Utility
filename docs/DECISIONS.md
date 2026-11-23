# Decisions Log

## 2026-09-21 — Initial code steward
**What:** Luke is the initial code steward for the team.
**Why:** Luke is the repo owner.

## 2026-10-20 - Define the problem as an incomplete handoff with two parts
**What:** We frame our problem as one problem, an incomplete handoff, instead of two separate problems. A handoff is complete only when it transfers both the Why (decisions and their reasons, for idea alignment) and the What (completed work, current status, and next steps). It covers both takeover and integration, and targets small university course project teams. Details are in `PROJECT_OVERVIEW.md`.
**Why:** Our interview evidence comes from a course project team whose problems appeared at integration and involved both misaligned decisions and unclear work status. A narrower, evidence-backed scope keeps the problem specific and avoids claims beyond our evidence.

## 2026-10-20 - Build the tool as an agent skill with PR-based review
**What:** The tool is an agent skill run through Codex or Claude Code in the team's repository. It drafts a Markdown handoff document from chat history, Git history, notes, and the giver's input. The giver corrects the draft and opens a PR, and the receiver comments until nothing is unclear, then approves.
**Why:** This keeps humans in charge of accuracy and of deciding when a handoff is complete, and uses the PR review flow the team already follows. The AI only drafts, links sources, and lists missing information instead of guessing.

## 2026-11-21 - Code Steward interation
What: We decide to take turns in order. Namely, everyone becomes code steward, review 2 PRs and merge it into main, then rotate to the next one. (Luke -> Eric -> Freddy -> 哲葦)
Why: The course requires every team member be code steward at least twice accross the whole project.

## 2026-11-22 - Shared handoff document template
**What:** All transfer-* skills use one handoff template, `skills/HANDOFF_TEMPLATE.md`, saved as `handoffs/YYYY-MM-DD-<topic>.md`. It has a short header (giver, receiver, date, scope) and seven sections: Decisions (numbered D1, D2, ... with Why, Rejected, and Source), Completed Work, Current Status, Next Steps, Open Questions (`- [ ]` unresolved, `- [x]` resolved), Review Record, and Quiz (`Pending` until `transfer-quiz` writes it). Writing rules and source formats are kept in a comment or in the skills, not in the visible template.
**Why:** The skills are split into modules, so they need one shared structure to read and write the same document. A short template keeps handoff documents easy to read. Numbered decisions and checkbox Open Questions let the skills refer to items and check whether the draft is ready.
**Alternative rejected:** Instructions inside every section of the template (makes each handoff document longer). Generating the quiz in the first draft (the giver corrects the draft first, so the quiz would test content that may change).
**Led by:** Eric

## 2026-11-22 - Design of the transfer-quiz skill
**What:** `transfer-quiz` is one skill with two modes. In Generate mode, the giver's agent drafts 1-5 questions on the most important points of the handoff document, mostly multiple choice with an `I'm not sure` option and open-ended only for easily confused points, and the giver approves them. The questions go in the Quiz section, and the answer key goes in a collapsed `<details>` block in the same section. In Take mode, the receiver's agent gives the quiz, explains each wrong answer, and asks the receiver to explain the point again in their own words, then judges whether they understood. If the document is unclear, or the receiver still does not understand, the agent drafts a PR comment asking the giver to improve the document. The result is recorded in the Review Record section before the receiver approves the PR.
**Why:** The receiver must understand the handoff the same way the giver intended, and a short, mostly multiple choice quiz checks this without taking much time. Restating in their own words shows understanding better than asking the same question again. Sending unclear parts back to the giver fixes the team's record instead of only the receiver's understanding.
**Alternative rejected:** Writing the quiz inside `transfer-prepare` (rejected to keep quiz logic in one place). Putting the answer key in a separate file (still visible in the PR, and adds files without hiding anything). Asking a rephrased question after an explanation (hard to ask the same concept in a different way). Having the giver confirm the receiver's understanding (the agent judges it, so the giver writes the document in more detail instead).
**Led by:** Eric

## 2026-11-22 - Code steward: Luke -> Eric
**What:** Eric takes over as code steward from Luke, following the rotation order in the 2026-11-21 entry (Luke -> Eric -> Freddy -> 哲葦). The change takes effect after Luke merges #9.
**Why:** Luke reviewed and merged 2 PRs, #11 (handoff template) and #9 (transfer-quiz skill), which completes his turn.
**Handoff notes:**
- #16 (transfer-prepare skill) is open and not reviewed yet. Eric reviews it as steward.
- No other pending items.
**Led by:** Eric

## 2026-11-23 - Implement handoff preparation as a single skill
**What:** I implement `transfer-prepare` as a single agent skill under `skills/transfer-prepare/`. It gathers information from chat history, Git history, meeting notes, and the giver's notes. The skill extracts the Why (decisions and reasons) and What (completed work, current status, and next steps), tracks sources, and marks missing or conflicting information as Open Questions instead of fabricating. It then generates a handoff draft following the shared `skills/HANDOFF_TEMPLATE.md`. The Quiz section is left as Pending for the separate `transfer-quiz` skill.
**Why:** Keeping evidence collection, analysis, and draft generation in one skill simplifies the workflow and avoids unnecessary intermediate steps. Using the shared template ensures consistent output across skills, while source references and Open Questions help the giver verify the draft before handing it to the receiver.
**Led by:** Che-Wei

## 2026-11-23 - 
**What:** Update and add some entries of PR template. The details are listed below
  * Discuss why we use this PR instead of only what
  * Specify the AI model we are using instead of only the tool name
  * Explpicitly describe how AI was used with input, output and what human did
  * Add a checklist to click before issuing the PR
  * Add a notes section for reviewer
**Why:** Lab 12 provide a PR template which includes some information that is valueable to be recorded and consider before issuing a PR
**Led by:** Luke
