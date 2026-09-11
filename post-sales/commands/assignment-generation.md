---
description: "Generates one open-ended, rubric-graded assignment for a session, weighted rubric, explicit starter-kit split, and setup/security checks for code-based assignments handled at creation time. Hands off to the existing acceler-post-sales:assignment-reviewer for the actual review pass."
argument-hint: "<client name + which day/session this assignment is for>"
---

Generate the assignment for this session.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/assignment-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, stop and ask.
3. If that day's code-demo already exists, build on the same tool or technique rather than a disconnected task.
4. Write the assignment against §2's rules, weighted rubric, explicit starter-kit split if one is provided, real scoring criteria for any subjective item, Apply/Analyze/Create Bloom's level.
5. If code-based, run the conditional checks: no real credentials or PII, no license-incompatible code, data files described, prerequisites and execution environment stated, starting references provided even for open-ended tasks.
6. Self-verify per §3: rubric weights sum to 100%, code-based checks run only if actually code-based.
7. Save to `Outputs/[Client]/assignment.docx`, plus a `starter-kit/` subfolder if one exists.
8. Hand off to `acceler-post-sales:assignment-reviewer` for the actual review pass, this command doesn't review its own output.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Weighted rubric present, weights sum to 100%
- [ ] Starter-kit split stated explicitly where applicable
- [ ] Subjective items have real scoring criteria, not tips alone
- [ ] Code-based conditional checks run only when applicable
- [ ] Saved to `Outputs/[Client]/assignment.docx`
- [ ] Handed to the existing `assignment-reviewer`, not reviewed inline here
