---
description: "PIPELINE STAGE 4 (post-sales). Builds a day-by-day instructor roster from a finalized Lesson Plan, reusing the existing acceler-presales:instructors query logic and Knowledge Graph directly, one query per day rather than one for the whole engagement. Avoids assigning the same instructor two days running by default."
argument-hint: "<client name>"
---

Finalize the instructor roster for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/instructor-finalization/SKILL.md` in full first.
2. Load `Outputs/[Client]/lesson-plan.xlsx`, approved. If it isn't approved yet, stop and say so, don't match against a draft.
3. For each day-tab, run the same query `acceler-presales:instructors` already runs against that day's Topic/Subtopic columns, post-sales deliverable branch only, `pre_sales_only` names excluded.
4. Assemble the day-by-day roster. Apply the consecutive-day default, don't assign the same instructor two days running unless it's genuinely needed and the instructor agrees, state any override and why.
5. Save to `Outputs/[Client]/instructor-roster.md`.

## Quality checklist (apply before presenting results)

- [ ] Ran against an approved Lesson Plan, not a draft
- [ ] One query per day against that day's actual topics, not a single engagement-wide query
- [ ] Deliverable branch used throughout, no `pre_sales_only` names surfaced
- [ ] Consecutive-day default applied, overrides stated explicitly with reason
- [ ] Saved to `Outputs/[Client]/instructor-roster.md`
