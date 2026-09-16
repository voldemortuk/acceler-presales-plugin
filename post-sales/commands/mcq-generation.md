---
description: "Generates one MCQ set for a session, applying NBME item-writing rules and objective alignment at creation time. Starts with two already-strong generation-learnings candidates (explanation completeness, distractor-text independence) from day one. Hands off to the existing acceler-post-sales:mcq-reviewer for the actual review pass."
argument-hint: "<client name + which day/session this MCQ set is for>"
---

Generate the MCQ set for this session.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/mcq-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, stop and ask.
3. Map each item to a stated Learning Objective from the relevant day's rows, confirm single-best-answer versus multi-select from how the objective/stem should be framed before writing distractors.
4. Write items against §2's rules, plausible distractors, no cueing, no compound claims, correct answer-key format.
5. Apply §3 from this first run: every explanation addresses why each wrong option is wrong, and no two options share near-identical phrasing.
6. Save to `Outputs/[Client]/mcq.docx`.
7. Hand off to `acceler-post-sales:mcq-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Every item traced to a stated objective, format (single-best vs multi-select) confirmed before writing
- [ ] No implausible distractors, cueing, compound claims, or length giveaways
- [ ] Every explanation addresses each wrong option, not just the correct one
- [ ] No two options share near-identical phrasing differing only in a trailing clause
- [ ] Saved to `Outputs/[Client]/mcq.docx`
- [ ] Handed to the existing `mcq-reviewer`, not reviewed inline here
