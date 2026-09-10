---
description: "PIPELINE GATE (Stage 1). Review one Learner Onboarding Form for whether it's tied to the program's stated outcome, skill-assessment relevance, and data proportionality — human-gated fix loop, PR-vocabulary verdict."
argument-hint: "<path to the onboarding form, and the Discovery Facts Sheet if available> + client/program name"
---

Review this onboarding form against `skills/onboarding-form-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/onboarding-form-review/SKILL.md` in full first.
2. Check for the Discovery Facts Sheet (`discovery-fit-review` output). If missing, say so and run §1.2-1.3 only — don't invent an outcome to check §1.1 against.
3. Launch the `acceler-post-sales:onboarding-form-reviewer` agent per §2.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds.
6. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] Discovery Facts Sheet checked for and used, or its absence explicitly flagged
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
