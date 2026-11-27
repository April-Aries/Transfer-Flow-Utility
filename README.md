# Transfer Flow Utility

Transfer Flow Utility is a human-in-the-loop tool that helps members of small course project teams hand off work completely, including both the reasons behind decisions and the current state of the work. It is a set of agent skills that run in Claude Code or Codex, inside the team's repository.

## Problem

Small university course project teams often divide the work at the start and coordinate through chat apps. When work moves from one member to another, either when a teammate takes over a piece of work or when parallel parts are combined at integration, important information gets lost:

- **Why (idea alignment):** the decisions behind the work and the reasons for them. Without this, members may understand the same thing differently.
- **What (work status):** what has been done, its current status, and what should happen next.

A handoff is complete only when both are transferred. See the [Project Overview](./docs/PROJECT_OVERVIEW.md) for interview evidence and details.

## How It Works

Two people take part: the **giver**, who hands off the work, and the **receiver**, who takes it over. The handoff is a Markdown document in `handoffs/`, reviewed in a pull request. The AI drafts and checks. The giver corrects the document, and the receiver decides when the handoff is complete.

| Step | Who | What happens | Skill |
|---|---|---|---|
| 1 | Giver | The agent drafts the handoff document from chat logs, Git history, notes, and the giver's input, and lists what it could not find as Open Questions | `transfer-prepare` |
| 2 | Giver | The giver corrects the draft and answers the Open Questions. The agent writes them into the same document after the giver approves the changes | `transfer-prepare` (Update mode), or by hand |
| 3 | Giver | The agent drafts 2-5 questions on the Why and the What. The giver answers them first, then approves the quiz | `transfer-quiz` (Generate mode) |
| 4 | Giver | Optional: the agent checks the document for missing sources, contradictions, and quiz problems | `transfer-review` |
| 5 | Giver | The giver commits the document, pushes, and opens a PR with the receiver as reviewer | (by hand) |
| 6 | Both | The receiver comments on unclear points in the PR. The agent reads the comments, writes the giver's answers into the same document, and drafts replies. The giver then revises the quiz. Repeat until nothing is unclear | `transfer-prepare` (Update mode), `transfer-quiz` (Generate mode, revision) |
| 7 | Receiver | The receiver takes the quiz. The agent explains wrong answers or drafts PR comments for unclear points, posting them only after the receiver confirms | `transfer-quiz` (Take mode) |
| 8 | Receiver | When the quiz has passed, the receiver approves the PR | (by hand) |

The full list of skills is in the [Skill Index](./skills/SKILL_INDEX.md), and the document format is in [`skills/HANDOFF_TEMPLATE.md`](./skills/HANDOFF_TEMPLATE.md).

## Human-AI Boundary

AI drafts the handoff document, links sources, and flags missing information and possible conflicts. Humans correct and complete the draft, resolve conflicts, and decide when a handoff is complete. The skills never commit, push, approve, or merge, and they post a PR comment only after the user confirms it. See the [Draft Evaluation Plan](./docs/EVALUATION_PLAN.md) for the full boundary.

## Requirements

- [Claude Code](https://code.claude.com/docs) or [Codex](https://developers.openai.com/codex)
- Git, and a GitHub repository where the team works
- [GitHub CLI](https://cli.github.com/) (`gh`), signed in. Optional: `transfer-quiz` uses it to find the PR base branch and to post PR comments. Without it, the agent gives you the comment text to post yourself.

## Install

The giver and the receiver both need the skills, so install them once in the team's repository and commit them.

1. Clone this repository next to your project, for example to `../Transfer-Flow-Utility`.
2. In the root of your project, copy the `skills/` folder. The skills read `skills/HANDOFF_TEMPLATE.md` at this path, so keep the folder at the repository root.
3. Copy the three skill folders to where your agent loads skills from: `.claude/skills/` for Claude Code, `.agents/skills/` for Codex. Copy to both if your team uses both.
4. Commit the copied folders.

macOS or Linux:

```bash
TFU=../Transfer-Flow-Utility
cp -r "$TFU/skills" .
mkdir -p .claude/skills .agents/skills
cp -r skills/transfer-prepare skills/transfer-quiz skills/transfer-review .claude/skills/
cp -r skills/transfer-prepare skills/transfer-quiz skills/transfer-review .agents/skills/
```

Windows (PowerShell):

```powershell
$TFU = "..\Transfer-Flow-Utility"
Copy-Item -Recurse "$TFU\skills" .
New-Item -ItemType Directory -Force .claude\skills, .agents\skills | Out-Null
Copy-Item -Recurse skills\transfer-prepare, skills\transfer-quiz, skills\transfer-review .claude\skills\
Copy-Item -Recurse skills\transfer-prepare, skills\transfer-quiz, skills\transfer-review .agents\skills\
```

To update the skills later, delete the old copies first, then copy again. Otherwise the new folders are copied inside the old ones.

To check the install, start a new session of your agent in the project root and ask "Which transfer skills are available?". It should list `transfer-prepare`, `transfer-quiz`, and `transfer-review`.

**To try the skills in this repository**, for example to run the tests, skip step 2 and copy only the skill folders (step 3). The copies in `.claude/skills/` and `.agents/skills/` are ignored by Git here, so `skills/` stays the only source.

## Usage

Start the agent in the root of the project. In Claude Code, call a skill with `/transfer-prepare`. In Codex, mention it with `$transfer-prepare`. You can also describe the task in plain words, for example "prepare a handoff for the data loader", and the agent picks the skill.

### Giver

1. **Draft the document.** Run `transfer-prepare` and say what you are handing off, to whom, and where the evidence is. For example:

   ```
   /transfer-prepare Hand off the data loader work on branch data-loader to Alice.
   Chat log: notes/chat-2026-11.txt. My notes: notes/handoff-notes.md.
   ```

   The draft is saved as `handoffs/YYYY-MM-DD-<topic>.md`. Export chat logs as `.txt` or `.md` files first, because the agent cannot read LINE or Discord directly.
2. **Correct the draft.** Check every item against its source. Run `transfer-prepare` again and tell the agent what is wrong and the answers to the Open Questions. It finds the existing draft, shows you every change before writing it into the same file, and marks the answered Open Questions as `- [x]`. You can also edit the file by hand.
3. **Write the quiz.** Run `transfer-quiz`. The agent shows the questions without the answers and asks you to answer them first. Where your answer differs from its draft, it fixes the answer key, rewrites the question, or asks you to add the missing information to the document. It writes the quiz only after you approve it.
4. **Check the document (optional).** Run `transfer-review` and fix the findings you agree with.
5. **Open the PR.** Commit the document, push, and open a PR with the receiver as reviewer. The PR contains the quiz from the start.
6. **Answer comments.** When the receiver comments, run `transfer-prepare`. It reads the receiver's comments on the PR (or the ones you paste), asks you what it cannot find in the sources, and updates the same document after you approve the changes. Then run `transfer-quiz` again so it revises only the affected questions, and commit and push. After you push, the agent posts the replies to the comments that you approved.

### Receiver

1. **Read and comment.** Read the handoff document in the PR, and comment on anything unclear or missing. Do not open the `<details>` block at the end of the Quiz section: it holds the answer key.
2. **Take the quiz.** Check out the PR branch and run `transfer-quiz`. Answer all questions in one reply, for example `Q1: B, Q2: C, Q3: <short explanation>`. The handoff document is open book. For each wrong answer, the agent either explains the point and asks you to restate it in your own words, or, if the document is unclear, drafts a PR comment for the giver that it posts only after you confirm.
3. **Record and approve.** The agent writes the result into the Review Record as **Passed** or **Waiting for giver**. Commit and push it. If it is Waiting for giver, run the quiz again after the giver updates the document. When it has passed and you have no other questions, approve the PR.

## Project Architecture

```
.
├── CONTRIBUTORS.md               # All contributors to this project
├── docs                          # Documents
│   ├── AI_USAGE_GUIDELINES.md
│   ├── DECISIONS.md
│   ├── EVALUATION_PLAN.md
│   └── PROJECT_OVERVIEW.md
├── README.md
└── skills                        # Source of the skills
    ├── HANDOFF_TEMPLATE.md       # Shared handoff document format
    ├── SKILL_INDEX.md            # What each skill does and when to use it
    ├── transfer-prepare
    │   └── SKILL.md
    ├── transfer-quiz
    │   └── SKILL.md
    └── transfer-review
        └── SKILL.md
```

## Project Documents

- [Project Overview](./docs/PROJECT_OVERVIEW.md): team charter, problem statement, and interaction design
- [AI Usage Guidelines](./docs/AI_USAGE_GUIDELINES.md)
- [Draft Evaluation Plan](./docs/EVALUATION_PLAN.md)
- [Decisions Log](./docs/DECISIONS.md)
- [Contributors](CONTRIBUTORS.md)
