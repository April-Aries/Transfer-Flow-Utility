---
name: transfer-prepare
description: Draft a source-backed handoff document when a giver wants to prepare project work for another teammate.
---

# Transfer Prepare

## Inputs

- Target project: Use the directory specified by the user. Otherwise, inspect the current working directory to identify the target project. If the target is unclear, ask the user rather than assuming that the Git repository root is the target project.
- Handoff scope: The task, feature, branch, or paths being handed off. Use an explicitly specified scope without asking for confirmation again. Only label the scope provisional when it must be inferred from incomplete information, and ask for confirmation after presenting the draft.
- Giver and receiver: Use the identities explicitly provided by the user. If either is missing, write `Unknown`; do not infer identities from Git authorship. If the user says a participant has not been decided, retain `Unknown` without asking for a name again.
- Additional materials: Project documents, source code, or other relevant materials not stored in Git.

## Workflow

### 1. Establish the handoff scope
Identify the target project and the handoff scope from the request and available project information. Once the target project is identifiable, proceed with the available evidence and collect missing information for one consolidated set of questions after the draft. Do not interrupt drafting to ask for missing participant identities or decision reasons. If the target project itself cannot be identified, ask the user before proceeding.

### 2. Inspect the project and materials
Start with the project directory structure and README. Then inspect documents, source code, Git history, and additional materials relevant to the agreed scope.

- Record sources for factual claims using the shared template's source formats.
- For a Git-backed project, record the inspected branch (or detached HEAD), HEAD commit, and whether the working tree has uncommitted changes. State whether the draft includes those changes; do not present the working tree as identical to the recorded commit when it differs. If Git metadata is unavailable, mark it as unavailable rather than inventing a version.
- Use the user-specified branch or commit as the primary version; otherwise, use the current working tree. Do not scan other branches by default. Consult another version only when requested or when a relevant source explicitly requires it to understand the handoff, and label that evidence with its ref or commit.
- Differences between branches or historical versions are not automatically conflicting claims. Raise a version-selection question only when choosing between versions materially affects the handoff and the request does not already resolve that choice.
- Treat PR status, review status, and ownership mentioned in commits or documents as historical records. Include the source date or commit and state that current status is unverified unless independently supported by current evidence. Local remote-tracking refs reflect the last fetch, not necessarily the current remote state.
- Distinguish completed work from unfinished work. Mark progress that no source confirms as "(reported, not verified)".
- Distinguish documented claims, observations from source code or skill instructions, and execution results. A README claim or an implementation's presence does not prove that a feature runs successfully. Describe execution as successful only when an available execution or test result supports that claim, citing the result and its version or scope when known. Otherwise, state that execution has not been verified for the inspected state.
- Do not infer a decision's reason from code alone. List missing reasons and other missing information as Open Questions rather than guessing.
- Include a decision and its reason in Decisions only when evidence establishes that they apply to the primary version. Do not transfer a rationale from a different implementation merely because the feature or file name matches. If applicability is uncertain and matters to the handoff, describe it in Open Questions rather than presenting it as an adopted decision.
- If sources conflict, preserve each claim with its source for the giver to clarify. Reading code alone does not establish that its behavior has been verified by execution.

### 3. Draft the handoff document
Read `../HANDOFF_TEMPLATE.md`, relative to this skill directory.

- Save the draft as `handoffs/YYYY-MM-DD-<topic>.md` under the target project directory.
- Preserve the template's section names and source formats. Fill in the header and the decisions, completed work, current status, and next steps from the available evidence.
- In Next Steps, distinguish source-backed remaining tasks from your own recommendations. Cite the source for established tasks. Include a recommendation only when it directly helps the handoff and label it "Suggested, not agreed"; do not present general repository cleanup or process improvements as assigned work.
- Record the inspected version and treatment of uncommitted changes in Scope or Current Status without adding template sections. Clearly label any provisional scope and keep missing participant identities as `Unknown`.
- Add missing or conflicting information to Open Questions using the template's unchecked items. Include each conflicting claim and its source.
- Keep questions focused on gaps that affect understanding, continuation, or verification of the handed-off work. Group related gaps, omit questions already answered by the request, and do not turn every undocumented alternative or incidental difference into a question.
- Leave `Corrected by` and `Confirmed by` as `Pending`. Leave the Quiz section pending for `transfer-quiz`.

### 4. Check and present the draft

- Check that the draft follows the shared template and that factual claims cite their sources.
- Cross-check claims against the available materials, including the giver's statements. Keep unresolved conflicts in Open Questions rather than changing the draft merely to agree with the giver.
- Present the draft's location, then ask the outstanding handoff-relevant questions together in one message. Reuse the Open Questions identifiers and avoid duplicate questions. Ask for missing identities only if the user has not already said they are undecided, and ask for scope confirmation only if the scope is provisional. Ask the giver to review the draft, correct errors, and supply missing information; leave unanswered items unresolved rather than repeatedly prompting during the initial draft.

## Boundaries

- Read project materials and create the handoff draft only. Do not modify project source code or other existing project files.
- If the intended output file already exists, stop and ask the user how to proceed rather than overwriting it.
- Do not run project code or tests as part of inspection; report execution results only when supported by the provided evidence.
- Do not generate quiz questions, commit, push, approve PRs, or merge PRs. Human review and quiz generation happen after this initial draft.
