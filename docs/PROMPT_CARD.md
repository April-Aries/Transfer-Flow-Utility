# Prompt Card

What the AI in Transfer Flow Utility does, what it is told to do, and where humans stay in control. The prompts are the `SKILL.md` files in `skills/`. This card summarizes them. When the card and a `SKILL.md` disagree, the `SKILL.md` is what the agent follows, and the card should be fixed.

## Overview

- **Where the AI runs:** in the user's own Claude Code or Codex session, inside the team's repository. The project does not call a model API itself, so the model is whatever the user's agent uses.
- **Prompts:** three skills, loaded by the agent when the user calls them or when the request matches their description. [`CLAUDE.md`](../CLAUDE.md) holds the rules that all skills share.
- **Shared format:** every skill reads and writes the handoff document defined in [`skills/HANDOFF_TEMPLATE.md`](../skills/HANDOFF_TEMPLATE.md).
- **What the AI never does:** decide that a handoff is complete, commit, push, approve, or merge. It posts a PR comment only after the user confirms it.

## `transfer-prepare`

Prompt: [`skills/transfer-prepare/SKILL.md`](../skills/transfer-prepare/SKILL.md)

| | |
|---|---|
| **Purpose** | Draft a handoff document that covers both the Why and the What, so the giver does not start from a blank page, and keep that same document up to date as the giver corrects it and answers the receiver's questions. |
| **Run by** | The giver, at the start of a handoff (Create mode), and after each round of corrections or receiver questions (Update mode). |
| **Input** | From the giver: the handoff scope, and who the giver and the receiver are. Chat logs, meeting notes, documents, and the giver's notes as `.txt` or `.md` files; Git history of the related branch, commits, or paths. In Update mode, also the existing document, the giver's answers, and the receiver's comments on the PR. |
| **Output** | Create mode: a new file `handoffs/YYYY-MM-DD-<topic>.md` with all seven template sections, and the Quiz section left as `Pending`. Update mode: changes to the same file, and draft replies to the PR comments. |

**What it is told to do**

- Look for an existing handoff document first, pick Create or Update mode, and tell the giver which one.
- Draft first, then ask all open questions in one message. Write `Unknown` for a giver or receiver that is not given, instead of guessing from Git authorship.
- Pair every fact with its source in the template's formats. Record the inspected branch, HEAD commit, and whether uncommitted changes are included.
- Ignore commits and messages that are not about the handoff.
- Tell reported progress, code observations, and execution results apart. Do not claim that something runs unless a result shows it.
- When a reason or rejected alternative is missing, write `Unknown` or add an Open Question instead of guessing. When sources conflict, list every claim with its source and do not pick one.
- Label its own suggestions in Next Steps as "Suggested, not agreed".
- In Create mode, stop instead of overwriting an existing file.
- In Update mode, edit only the affected parts of the same document, keep the existing IDs, never write an answer the giver did not give and no source supports, and show every change before writing it. Post replies to PR comments only after the giver confirms them and has pushed the update.

**Human control points:** the giver chooses the scope and the inputs, approves every change to the document, answers the Open Questions and the receiver's questions, and confirms each PR reply before it is posted.

## `transfer-quiz`

Prompt: [`skills/transfer-quiz/SKILL.md`](../skills/transfer-quiz/SKILL.md)

| | |
|---|---|
| **Purpose** | Check that the receiver understood the handoff the way the giver meant it, before the receiver approves the PR. |
| **Run by** | The giver (Generate mode) after correcting the draft and after each later change; the receiver (Take mode) after reading the document. |
| **Input** | The handoff document. |
| **Output** | Generate mode: the Quiz section, with an answer key in a `<details>` block. Take mode: a Quiz result in the Review Record (`Passed` or `Waiting for giver`), and PR comments for the giver. |

**What it is told to do: Generate mode**

- Do not start while Open Questions are unresolved, unless the giver says they do not matter to the receiver.
- Write 2-5 questions on the points where a misunderstanding would do real damage: decisions and their reasons first, then current status, next steps, and easily confused points. At least one question on the Why and one on the What. No trivia, and no questions added just to reach five.
- Label each question with its topic, the section its answer comes from, such as `Why - Decisions`.
- Use multiple choice for at least half of the questions, always with an `I'm not sure` option.
- Every answer must be supported by the document alone. If it is not, ask the giver to add the information to the document instead of hiding it in the answer key.
- Show the giver the questions without the answer key and ask the giver to answer them first. Resolve every difference by fixing the key, rewriting the question, or asking the giver to add to the document. Write the quiz only after the giver approves it.
- When the document changes, revise only the affected questions and raise their revision number.

**What it is told to do: Take mode**

- Show all questions at once, without hints, and never show the answer key before the receiver answers.
- Clarify the wording of a question at most once. Do not answer questions about the content until the answers are in.
- For each wrong answer, first decide whether the receiver misunderstood or the document is unclear.
- If the receiver misunderstood: explain with a quote from the document, then ask the receiver to restate the point in their own words. Explain at most twice.
- If the document or the question is unclear, the receiver still does not understand, or the receiver asks for the giver: draft a PR comment for the giver, post it only after the receiver confirms, and record the reason (`document unclear`, `question unclear`, `still not understood`, or `receiver asked`).
- Never treat an unclear document as the receiver's mistake.

**Human control points:** the giver owns the questions and the answer key. The receiver confirms every PR comment and decides whether to approve the PR.

## `transfer-review`

Prompt: [`skills/transfer-review/SKILL.md`](../skills/transfer-review/SKILL.md)

| | |
|---|---|
| **Purpose** | An extra check of the handoff document before it is approved, to catch errors that the giver and the receiver might miss. |
| **Run by** | The giver or the receiver, after the quiz is written and before the PR is approved. |
| **Input** | The handoff document, its cited sources, Git history, and the template. |
| **Output** | Findings in the chat only, each with an ID (R1, R2, ...), location, issue, evidence, impact, and suggested fix, ranked as Blocker, Warning, or Suggestion. An overall status: `NEEDS CORRECTION`, `NEEDS VERIFICATION`, or `READY FOR HUMAN REVIEW`. It does not change the document. |

**What it is told to do**

- Check the structure against the actual template file.
- Check that each cited source exists and supports its claim. Classify each claim as Supported, Unsupported, or Not verified, and prefer Not verified when the evidence is insufficient. A commit message alone does not prove that a feature works.
- Look for decisions without reasons, unverified completion claims, missing blockers, vague next steps, Open Questions marked resolved without an answer, and exposed secrets.
- Check that quiz questions match the document and do not depend on unresolved Open Questions. If the quiz is still `Pending`, report that the quiz review was not done.
- Never give `READY FOR HUMAN REVIEW` when the quiz is missing or a check could not be completed.
- Use read-only inspection. Do not run project scripts or modify code.

**Human control points:** the giver decides which findings to fix. The result is a report, not an approval.

## Known failure modes

To be added after the test runs in #38 and the team's review of the results.
