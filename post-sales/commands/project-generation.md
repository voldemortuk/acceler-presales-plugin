---
description: "Generates one capstone/multi-milestone project, milestone/trap-graded or documentation-brief shape, whichever the engagement calls for. Builds the compliance-vs-verified-enforcement distinction in from day one, not just a compliance check. Hands off to the existing acceler-post-sales:project-reviewer for the actual review pass."
argument-hint: "<client name + which program stage this capstone is for>"
---

Generate the capstone project for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/project-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, stop and ask.
3. Choose the shape, milestone/trap-graded or documentation-brief, from the engagement's signal, not by default.
4. Build against §2's rules for that shape. If milestone/trap-graded, plant traps and write their forced-and-observed enforcement criteria together, per §3, never a compliance-only check.
5. Self-verify per §4: for the milestone/trap shape, actually build the reference solution and run its test suite, confirm it passes. For the documentation-brief shape, confirm a role-relevance mapping exists if the cohort spans multiple functions.
6. Save to `Outputs/[Client]/project/`.
7. Hand off to `acceler-post-sales:project-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Correct shape chosen from the engagement's signal, not defaulted
- [ ] Every planted trap has a forced-and-observed enforcement criterion, not compliance-only
- [ ] Reference solution built and its test suite actually run, for the milestone/trap shape
- [ ] Role-relevance mapping present for a multi-function cohort
- [ ] No real credentials, PII, or license-incompatible code
- [ ] Saved to `Outputs/[Client]/project/`
- [ ] Handed to the existing `project-reviewer`, not reviewed inline here
