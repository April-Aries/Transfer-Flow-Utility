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

## 2026-11-22 - Code steward: Luke -> Eric
**What:** Eric takes over as code steward from Luke, following the rotation order in the 2026-11-21 entry (Luke -> Eric -> Freddy -> 哲葦). The change takes effect after Luke merges #9.
**Why:** Luke reviewed and merged 2 PRs, #11 (handoff template) and #9 (transfer-quiz skill), which completes his turn.
**Handoff notes:**
- #16 (transfer-prepare skill) is open and not reviewed yet. Eric reviews it as steward.
- No other pending items.
**Led by:** Eric
