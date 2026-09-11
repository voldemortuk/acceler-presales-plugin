---
description: "Generates the Learner Onboarding Form for an engagement, one common baseline tweaked per client. Includes the lead-only conditional section inline when applicable, not as a separate form. Hands off to the existing acceler-post-sales:onboarding-form-reviewer for the actual review pass."
argument-hint: "<client name>"
---

Generate the onboarding form for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/onboarding-form-generation/SKILL.md` in full first.
2. Load the mandatory input per §1: `Outputs/[Client]/discovery-facts-sheet.md`, approved. If missing, stop and ask.
3. Build the baseline per §2: identity, the outcome-tie question sourced from the Facts Sheet, the concern question, a skill-matrix scoped to this cohort's actual tools, topics of interest, the data-proportionality reassurance.
4. Include the real workflow/repetitive-task questions per §3.
5. Include the lead-only conditional section per §4 only if this engagement has a distinguishable lead/manager audience, clearly marked optional.
6. Calibrate technical depth to the audience, from the Facts Sheet, not defaulted.
7. Self-verify per §5: outcome-tie question and skill-matrix list both actually built from this engagement's real Facts Sheet, not a template.
8. Save to `Outputs/[Client]/onboarding-form.docx`.
9. Hand off to `acceler-post-sales:onboarding-form-reviewer` for the actual review pass, this command doesn't review its own output.

## Quality checklist (apply before presenting results)

- [ ] Mandatory input loaded, or the run stopped and asked
- [ ] Outcome-tie and concern questions present, outcome-tie built from the real Facts Sheet
- [ ] Skill-matrix scoped to this cohort's actual tools, not copied wholesale
- [ ] Lead-only section included and clearly optional only when applicable
- [ ] Depth calibrated to audience, not defaulted
- [ ] Saved to `Outputs/[Client]/onboarding-form.docx`
- [ ] Handed to the existing `onboarding-form-reviewer`, not reviewed inline here
