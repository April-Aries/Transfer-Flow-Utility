# Transfer Flow Utility

Transfer Flow Utility is a planned human-in-the-loop tool that helps members of small course project teams hand off work completely, including both the reasons behind decisions and the current state of the work.

## Problem

Small university course project teams often divide the work at the start and coordinate through chat apps. When work moves from one member to another, either when a teammate takes over a piece of work or when parallel parts are combined at integration, important information gets lost:

- **Why (idea alignment):** the decisions behind the work and the reasons for them. Without this, members may understand the same thing differently.
- **What (work status):** what has been done, its current status, and what should happen next.

A handoff is complete only when both are transferred. See the [Project Overview](PROJECT_OVERVIEW.md) for interview evidence and details.

## Approach

The tool will be an agent skill, run through Codex or Claude Code in the team's repository. It will:

1. Draft a structured handoff document from chat history, Git history, notes, and the input of the person handing off.
2. Cover decisions and their reasons, completed work, current status, and next steps, linking each item to its source and listing missing information as open questions.
3. Ask the person handing off to correct the draft, fill in the gaps, and open a PR.
4. Let the receiver comment until nothing is unclear, then approve the PR, making the document the team's agreed record.

## Human-AI Boundary

AI drafts the handoff document, links sources, and flags missing information and possible conflicts. Humans correct and complete the draft, resolve conflicts, and decide when a handoff is complete. See the [Draft Evaluation Plan](EVALUATION_PLAN.md) for the full boundary.

## Project Documents

- [Project Overview](PROJECT_OVERVIEW.md): team charter, problem statement, and interaction design
- [AI Usage Guidelines](AI_USAGE_GUIDELINES.md)
- [Draft Evaluation Plan](EVALUATION_PLAN.md)
- [Decisions Log](DECISIONS.md)
- [Contributors](CONTRIBUTORS.md)
