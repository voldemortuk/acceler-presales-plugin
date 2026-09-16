---
description: "PIPELINE STAGE 3 (post-sales). Generates the internal, minute-level Lesson Plan (Topic/Objective/Subtopic/Time/Flow/Demo/Tools table, one tab per day) from deep research and the proposal's day-by-day section. Self-checks its own timing math, then hands off to the existing acceler-post-sales:lesson-plan-review for the actual review pass."
argument-hint: "<client name, plus a similar past Lesson Plan to reference if you have one>"
---

Generate the Lesson Plan for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/lesson-plan-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/deep-research.md` (approved) and the pre-sales proposal's day-by-day section. If either is missing, stop and ask.
3. Pull in the existing curriculum/topic library where available to fill in rows, and a similar past Lesson Plan if one was pointed to, for formatting and depth reference.
4. Build the table per §2's structure rules, one tab per day, seven columns per row, correct handling of break/lunch/AMA rows, pre/live/post-class segmentation, objectives matched to demo content, demo weight kept light ahead of any dedicated hands-on day.
5. Calibrate row density and demo depth to the audience signal from deep research's Facts Sheet, don't apply one dense template regardless of audience.
6. Self-verify per §3: compute each day's row-duration sum and confirm it matches that day's stated session length. Fix before proceeding if it doesn't.
7. Save to `Outputs/[Client]/lesson-plan.xlsx`.
8. Hand off to `acceler-post-sales:lesson-plan-review` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, confirmed happening on a real run of this exact command, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Seven-column structure correct, break/lunch/AMA rows handled as exceptions, not gaps
- [ ] Pre-class, live-class, post-class present as distinct rows per module
- [ ] No orphaned objectives, demo content matches or exceeds each row's Bloom's verb
- [ ] Row-duration math self-checked against stated day length before saving
- [ ] Saved to `Outputs/[Client]/lesson-plan.xlsx`
- [ ] Handed to the existing `lesson-plan-review`, not reviewed inline here
