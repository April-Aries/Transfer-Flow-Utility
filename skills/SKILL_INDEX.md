# Skill Index

The skills of Transfer Flow Utility. Each skill reads and writes only the handoff document in `handoffs/`, in the format of [`HANDOFF_TEMPLATE.md`](./HANDOFF_TEMPLATE.md). None of them commits, pushes, approves, or merges. See the [README](../README.md) for installation and the full flow.

| Skill | Run by | When | Reads | Writes |
|---|---|---|---|---|
| [`transfer-prepare`](./transfer-prepare/SKILL.md) | Giver | Starting a handoff | Chat logs, Git history, meeting notes and documents, the giver's notes | A new draft `handoffs/YYYY-MM-DD-<topic>.md`, with sources, Open Questions, and the Quiz section left as `Pending` |
| [`transfer-quiz`](./transfer-quiz/SKILL.md), Generate mode | Giver | After the giver has corrected the draft. Run it again after the document changes (see [Usage](../README.md#giver)) | The handoff document | The Quiz section: 2-5 questions with topics and an answer key, written after the giver answers and approves the quiz |
| [`transfer-review`](./transfer-review/SKILL.md) | Giver or receiver | After the quiz is written, before the PR is approved | The handoff document, its cited sources, the template | Nothing. It reports findings as Blocker, Warning, or Suggestion in the chat |
| [`transfer-quiz`](./transfer-quiz/SKILL.md), Take mode | Receiver | After reading the handoff document in the PR | The handoff document | A Quiz result in the Review Record (Passed or Waiting for giver), and PR comments to the giver after the receiver confirms them |

`transfer-quiz` picks its mode from the request, or from the document: the Quiz section and whether the user is the Giver or the Receiver.
