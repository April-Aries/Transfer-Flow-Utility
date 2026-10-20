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
