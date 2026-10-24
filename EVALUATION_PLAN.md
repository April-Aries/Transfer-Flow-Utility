# Draft Evaluation Plan

## Problem Grounding

### Who specifically has the collaboration problem we are addressing?

- Members of small university course project teams, about three to five people, who divide the work, work mostly on their own, and then hand work to a teammate or combine their parts at integration. See the Problem Statement in `PROJECT_OVERVIEW.md` for details and interview evidence.

### What do they currently do instead of our tool?

- They coordinate through chat apps such as LINE or Discord, a planning document written at the start, and occasional meetings.
- They hand work over through verbal explanations or messages of inconsistent quality, where the reasons behind decisions, the current status, and the next steps may be unclear or missing.

### What would be observably different if our tool worked?

- The receiving member would correctly describe the decisions behind the work, the reasons for them, the current status, and the next step.
- The receiving member would need fewer clarification questions to reach that understanding.

## Evaluation Plan Draft

### Success Definition

We will know our tool works if, compared with the baseline condition, the receiver achieves an equal or higher quiz score, asks an equal or lower number of clarification questions, and completes the handoff in less total time, while the final handoff document contains no critical factual errors.

### Target Users

- **Who:** Four university students outside our team who have worked on a course team project, tested as two pairs. In each pair, one person acts as the giver and the other as the receiver.
- **How we will reach them:** Through our personal networks, such as classmates from other courses and friends in other departments, starting with the people we interviewed for the Problem Statement if they are willing. Each team member is responsible for inviting at least one participant.

### Method

**Design.** Each pair completes two handoff sessions: one with our tool and one without it (the baseline). In the baseline, the giver manually completes the same structured handoff template used in the tool condition, without assistance from the Transfer Flow Utility or another AI tool. In both conditions, the giver then conducts a brief handoff meeting with the receiver for approximately 10 minutes.

| Pair | Session 1 | Session 2 |
|---|---|---|
| Pair 1 | Scenario X, baseline | Scenario Y, with tool |
| Pair 2 | Scenario X, with tool | Scenario Y, baseline |

**Materials.** We will prepare two simulated project scenarios of similar size and difficulty. Each scenario includes chat history, Git history, and notes, and contains a fixed answer key: the decisions and their reasons, the completed work, the current status, and the next step.

**Procedure for each session**

1. The giver studies the scenario materials so they can act as the person who did the work.
2. The giver hands off the work to the receiver, with or without the tool.
3. The receiver answers a short quiz about the decisions, reasons, status, and next step, without help from the giver.
4. Both participants rate how easy the handoff was to communicate and understand, on a scale from 1 (very hard) to 5 (very easy).
5. We hold a short interview with both participants about what was clear, what was missing, and how the handoff felt. After their second session, we also ask which of the two handoffs felt easier and why.

**Metrics**

- Receiver's quiz score, graded by team members against the answer key
- Number of clarification questions the receiver asks during the handoff
- Number of factual errors in the final handoff document, and how many AI draft errors the giver caught during review
- Total handoff time, including document preparation, review and correction, and the brief handoff meeting
- Ease ratings from both participants, and which session they preferred
- Participants' comments from the interview

**Timeline**

| Week | Activity |
|---|---|
| 9-12 | Build the tool; prepare the two scenarios and the quiz |
| 11-12 | Invite participants and schedule sessions |
| 12 | Pilot run with team members to fix problems in the materials |
| 13-14 | Run the sessions with both pairs |
| 15 | Analyze the results and report them in CP3 |

### Minimum Evidence Threshold

We will consider the tool successful only if all of the following hold:

- In both pairs, the receiver achieves an equal or higher quiz score with the tool than in the baseline condition.
- In both pairs, the receiver asks an equal or lower number of clarification questions with the tool than in the baseline condition.
- In both pairs, the total handoff time is shorter with the tool than in the baseline condition.
- No final handoff document produced with the tool contains a critical factual error, such as a wrong decision, a wrong reason, or a wrong status.

If any of these fail, we will treat the tool as not yet working and use the session recordings and interviews to find out why.

**Supporting evidence.** Participants' experience matters to us: a tool that makes handoffs feel easier to communicate and understand is more likely to be used. We will report the ease ratings, which session each participant preferred, and their reasons, as supporting evidence alongside the criteria above. They are not pass criteria because, with four participants, ratings alone cannot distinguish a real improvement from personal preference. 

With only two pairs, these results can show whether the tool works in realistic handoffs, but they cannot show that the effect holds for all teams. We will report the raw results for every session so readers can judge them directly.

### Human-AI Boundary

| The AI does | Humans decide |
|---|---|
| Collects the provided inputs and drafts the handoff document | Which inputs to provide |
| Links each item to its source | Whether each item is correct, and what to change |
| Lists information it cannot find as open questions, instead of guessing | The answers to the open questions |
| Flags decisions that appear to conflict across handoff documents | Which choice to keep when a conflict is real |
| | Whether the handoff is complete (the receiver approves the PR) |

The AI never approves a handoff, fills a gap by guessing, or resolves a conflict on its own. In the evaluation, quiz answers are graded by team members against the answer key, not by AI.
