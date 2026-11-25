---
name: transfer-quiz
description: Check that the receiver of a handoff understands it the way the giver intended. Use in two cases. (1) The giver has finished a handoff document and wants to write its quiz: draft 2-5 questions with an answer key and add them to the document. (2) The receiver has read a handoff document and wants to take its quiz: give the questions, check the answers, explain mistakes until the receiver understands, send unclear parts back to the giver, and record the result. Triggers include "write the quiz", "generate handoff quiz", "take the quiz", "check my understanding of this handoff".
---

# Transfer Quiz

A handoff transfers two things: the **Why** (decisions and their reasons) and the **What** (completed work, current status, next steps). This skill checks that the receiver understood both the way the giver meant them, before the receiver approves the handoff PR.

The skill has two modes:

- **Generate mode**, run by the **giver**: draft the quiz and its answer key, and write them into the handoff document after the giver approves.
- **Take mode**, run by the **receiver**: give the quiz, check the answers, make sure every mistake ends in real understanding, and record the result.

## Step 0: Choose the mode and find the document

1. Decide the mode from the user's request. If it is unclear whether the user is the giver or the receiver, ask. Do not guess.
2. Find the handoff document. Handoff documents live in `handoffs/` and are named like `handoffs/2026-11-02-training-pipeline.md`.
   - If the user names a file or a PR, use it.
   - Otherwise, list the handoff files changed on the current branch, for example with `git diff --name-only origin/main...HEAD -- handoffs/`. If exactly one file is found, confirm it with the user. If none or several are found, ask the user which one.
3. Read the whole document before doing anything else. Its structure follows `skills/HANDOFF_TEMPLATE.md`.

## Generate mode (giver)

### G1. Check that the document is ready

The quiz tests the final content of the handoff, so the content must be settled first.

- If the **Open Questions** section still has unresolved items (`- [ ]`), tell the giver which ones and ask them to answer those first. Continue only if the giver says the remaining items are not important for the receiver.
- If the Quiz section already has questions, instead of the `Pending` placeholder, this is a revision. Follow G6 instead of writing a new quiz from scratch.

### G2. Choose what to test

Test only what matters. Pick the points where a misunderstanding would cause real damage, in this order of priority:

1. **Decisions and their reasons (Why).** Especially decisions that look counterintuitive, or where a rejected alternative also looks reasonable.
2. **Current status.** Especially known issues, and parts that look finished but are not.
3. **Next steps.** Especially what the receiver must do first, or must not do.
4. **Easily confused points.** Terms, settings, or names that could be read in more than one way.

Write **2 to 5 questions**, with at least one question on the Why and at least one on the What. A handoff must transfer both, so the quiz must check both. Two good questions are better than five weak ones. Never add questions just to reach five. Do not test trivia, such as file names, dates, or exact numbers that the receiver can look up when needed, unless getting them wrong would break the work.

If the Decisions section is empty, tell the giver that the handoff has no Why and ask them to add the decisions behind the work. Write only What questions if the giver confirms that there are no decisions to hand off.

Give each question a **topic**, the section of the document that its answer comes from:

- `Why - Decisions`
- `What - Completed Work`
- `What - Current Status`
- `What - Next Steps`

An easily confused point takes the topic of the section it comes from. If the answer uses more than one section, use the section the question is mainly about. The topic is shown with the question and in the Quiz result, so that the giver and the receiver can see which part of the handoff each result is about.

### G3. Choose the question type

- **Multiple choice is the default.** It is fast to answer and to check.
- **Open-ended** is only for easily confused points, where the receiver must explain a reason in their own words, or where the options of a multiple choice question would give the answer away.
- At least half of the questions should be multiple choice.

### G4. Write the questions and the answer key

Rules for every question:

- **Every answer must be supported by the document alone.** The receiver only has the document. If the correct answer depends on information that is not in the document, the document has a gap: tell the giver what is missing and ask them to add it to the document, instead of hiding the information in the answer key.
- Every answer key entry cites its **source**: the section and item of the handoff document it comes from.
- Ask about meaning, not wording. Do not copy sentences from the document into the question, so that the receiver cannot answer by matching words.

Rules for multiple choice questions:

- 3 or 4 real options, exactly one correct, plus a last option `I'm not sure`.
- Wrong options must be plausible. Good sources are rejected alternatives from the Decisions section and likely misreadings of the document.
- Do not use "All of the above" or "None of the above".
- Vary the position of the correct option across questions.

Rules for open-ended questions:

- Ask for one specific thing, such as the reason for one decision.
- In the answer key, list the **key points** the answer must contain. An answer is correct only if it covers every key point in substance, in any wording.

### G5. Review with the giver, then write

1. Show the giver the full draft: questions, topics, options, answer key, and sources.
2. Ask the giver to check each question: Is the answer key correct? Is this point really important? Is anything important missing? The giver may change, add, or remove questions. The giver's version is final, as long as it still follows G2 and G3. If a change leaves fewer than two questions, or no question on the Why or the What, tell the giver and ask how to fix it.
3. Only after the giver approves, replace the content of the Quiz section with the quiz, using the format in [Quiz format](#quiz-format). Do not change any other section of the document.
4. Tell the giver to commit and push the change to the handoff PR. Do not commit or push unless the giver asks you to.

### G6. Revise an existing quiz

Run this when the giver has changed the document, for example after a receiver's quiz exposed an unclear part.

1. Find which questions are affected by the change. Look at the latest **Quiz result** in the Review Record section to see which questions were sent back to the giver.
2. Rewrite only the affected questions, and add new questions only if the change added an important new point. Keep the total between 2 and 5, with at least one question on the Why and one on the What.
3. Increase the quiz revision number by one, and set the `Revision` of each changed or new question to the new number. Do not change the revision of untouched questions.
4. Review with the giver and write, as in G5.

## Take mode (receiver)

### T1. Prepare

1. Check that the Quiz section has questions. If it still says `Pending`, tell the receiver that the giver has not written the quiz yet, and stop.
2. Ask the receiver whether they have read the whole handoff document. If not, ask them to read it first. They may keep the document open while answering. The quiz checks understanding, not memory.
3. Decide which questions to give:
   - If the Review Record section has no Quiz result yet, give every question.
   - If it has a previous Quiz result, give only the questions that were not passed in that result, and the questions whose `Revision` is higher than the revision recorded in that result.
4. Read the answer key yourself. **Never show the answer key, the correct options, or the sources to the receiver before they have answered.**

### T2. Give the quiz

1. Show all selected questions in one message, with their options, exactly as written in the document. Do not add hints.
2. Ask the receiver to answer all of them in one reply, for example `Q1: B, Q2: C, Q3: <short explanation>`.
3. If the receiver asks what a question means, you may clarify the wording of the question. Do not answer questions about the content of the handoff until the receiver has submitted their answers.

### T3. Check the answers

- **Multiple choice:** correct only if the chosen option matches the answer key. `I'm not sure` counts as not correct.
- **Open-ended:** correct only if the answer covers every key point in the answer key, in any wording. If some key points are missing or wrong, the answer is not correct.

Tell the receiver their result for every question, with its topic, then handle each question that is not correct as described in T4.

### T4. Handle each wrong answer

First decide **why** the answer is wrong. Compare the receiver's answer with the document text, not only with the answer key.

- **The document is unclear** if any of these is true:
  - the document text does not clearly support the answer key,
  - the receiver's answer is a reasonable reading of the document text,
  - the document contradicts itself, or contradicts the answer key.
- **The receiver misunderstood** if the document states the point clearly and the receiver missed or misread it.

If you cannot tell which one it is, ask the receiver how they arrived at their answer, then decide.

**If the receiver misunderstood:**

1. Explain the point. Quote or point to the passage in the document that supports the correct answer, explain why the correct answer is right, and explain why the receiver's answer is wrong.
2. Ask the receiver to explain the point again **in their own words**. Do not accept a copy of your explanation or of the document. If the answer only repeats wording, ask them to put it differently or to apply it to the next step of the work.
3. Judge the restatement against the key points or the correct option. If it is correct, the question is **passed after explanation**.
4. If the restatement is still wrong, explain once more in a different way, and ask for one more restatement.
5. If the receiver says they still do not understand, or the second restatement is still wrong, send the question to the giver as described below. The giver should explain the point in more detail in the document.

**If the document is unclear, or the receiver still does not understand after explanation:** send the question to the giver.

1. Draft a PR comment for the giver that states:
   - which question it is,
   - which part of the document caused the problem, quoted,
   - how the receiver understood it,
   - for an unclear document: what is unclear; for a receiver who still does not understand: what the receiver still does not understand.
2. Show the draft to the receiver. Post it with `gh pr comment <PR number> --body-file <file>` only after the receiver confirms. If `gh` is not available or the PR is unknown, give the receiver the text to post themselves.
3. Mark the question as **sent to giver**. The receiver takes this question again after the giver updates the document and revises the quiz (G6).

Do not explain an unclear point to the receiver as if the receiver made the mistake. The fix belongs in the document, so that the team's record is correct.

### T5. Record the result

1. Add a Quiz result to the **Review Record** section using the format in [Quiz result format](#quiz-result-format). Do not change any other section of the document, and do not remove earlier Quiz results.
2. The quiz is **passed** only if every given question is either correct on the first answer or passed after explanation. If any question was sent to the giver, the status is **Waiting for giver**.
3. Tell the receiver to commit and push the result to the handoff PR. Do not commit or push unless the receiver asks you to.
4. Tell the receiver the next step:
   - **Passed:** the receiver may approve the PR when they have no other questions. Approving is the receiver's decision.
   - **Waiting for giver:** wait for the giver to update the document and the quiz, then run Take mode again.

You never approve or merge the PR yourself.

## Quiz format

The Quiz section shows the questions to everyone. The answer key goes in a collapsed block at the end of the same section.

````markdown
## Quiz

Quiz revision: 1

Take this quiz with the `transfer-quiz` skill before approving this PR. Do not open the answer key below before taking the quiz.

### Q1 (Why - Decisions, multiple choice, Revision 1)

Why does the training script use a batch size of 16 instead of 32?

- A. The model converges faster with smaller batches.
- B. A batch size of 32 runs out of memory on the team's GPU.
- C. The original paper uses 16.
- D. I'm not sure

### Q2 (What - Next Steps, open-ended, Revision 1)

In your own words, what must you check before you run the next training job, and why?

<details>
<summary>Answer key (for the quiz agent only, do not open before taking the quiz)</summary>

### Q1

- **Answer:** B
- **Source:** Decisions > D1
- **Explanation:** 32 was tried first and ran out of memory on the shared GPU, so 16 was chosen. Faster convergence (A) was not a reason.

### Q2

- **Key points:**
  1. The data path in `config.yaml` still points to the old dataset.
  2. Training on the old dataset would waste the GPU time the team booked.
- **Source:** Current Status > item 2; Next Steps > item 1
- **Explanation:** The dataset was moved, but the config was not updated yet.

</details>
````

## Quiz result format

Add one block per quiz attempt at the end of the Review Record section.

````markdown
### Quiz result: 2026-11-02

- **Receiver:** <name>
- **Quiz revision taken:** 1
- **Status:** Passed | Waiting for giver

| Question | Topic | First answer | Outcome |
|---|---|---|---|
| Q1 | Why - Decisions | Correct | Passed |
| Q2 | What - Next Steps | Wrong | Passed after explanation |
| Q3 | What - Current Status | I'm not sure | Sent to giver: document unclear (<PR comment link>) |

**Notes:** <one line per explained or sent-back question: what the receiver misunderstood, or what is unclear in the document>
````

## Boundaries

- The giver owns the questions and the answer key. You draft them, and the giver approves them.
- In Take mode, you judge whether the receiver understood. The giver does not need to confirm it.
- You never fill a gap in the document by guessing, never show the answer key before the receiver answers, never edit sections other than Quiz and Review Record, and never approve or merge the PR.
