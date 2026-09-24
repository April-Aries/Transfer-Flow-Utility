# Draft AI Usage Guidelines

## Section 1 - What AI tools we plan to use, and what we will use each for

- We will ask Codex to evaluate team-written skills for correctness, clarity, missing edge cases, and consistency with project requirements. We will not ask Codex to generate a complete skill from scratch.
- We will use Codex to test our skills with representative inputs and identify cases where they do not work as intended.
- We will use Codex to summarize team discussions, decisions, and work handoff information.
- Codex will not decide whether the team has reached a consensus, assign responsibilities, or decide whether a handoff is complete. Team members must verify and approve the summary.

## Section 2 - How we will document AI interactions

- When an AI interaction affects repository code, documentation, or a project decision, we will document the following fields in the prompt engineering log:
  - The AI tool used
  - The prompt submitted
  - A summary of the AI response
  - The actions taken after receiving the response
  - The reason for taking those actions
- One-off questions do not require a log entry if their output does not affect the repository or a project decision.
- We will add an entry to `DECISIONS.md` when:
  - A new concern arises and requires a team decision
  - The problem or solution changes
  - The organization of the team changes, such as choosing the Code Steward
- Every pull request must state whether AI was used, how it was used, and how a human verified the result.

## Section 3 - How we will handle disagreements about AI output quality

- If two or more teammates disagree with AI-generated code or an AI-generated idea and provide evidence, the output will be paused and reviewed.
- Evidence may include test results, project requirements, technical documentation, or a reproducible example. Personal preference or voting alone does not count as evidence.
- The output will be rejected if it fails tests, conflicts with project requirements, contains factual errors, or cannot be explained by the contributor.
- If the disagreement remains after reviewing the evidence, the Code Steward makes the final decision for both code and feature ideas.
- The Code Steward's final decision and reason will be recorded in `DECISIONS.md`.
