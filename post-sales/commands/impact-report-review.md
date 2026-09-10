---
description: "POST-DELIVERY quality gate. Review one stakeholder-facing day-N impact report for attribution integrity (real quotes attributed to real named learners), internal numeric consistency, and padding discipline — human-gated fix loop, PR-vocabulary verdict. Companion to /acceler-post-sales:session-recap-review."
argument-hint: "<path to the report HTML, plus the chat log/transcript and KPI source numbers for verification> + client/program/day"
---

Review this impact report against `skills/impact-report-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/impact-report-review/SKILL.md` in full first.
2. Launch the `acceler-post-sales:impact-report-reviewer` agent per §2 — trace every tier-evidence quote to the real chat/transcript source, not just plausibility.
3. Any §1.1 (attribution integrity) finding gets flagged high-severity and escalated to the human immediately — do not run it through the normal 2-round fix loop.
4. Other findings follow the normal fix loop via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds.
5. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] Every tier-evidence quote traced to a real source, not assumed
- [ ] Every present learner in every tier has a specific, non-generic note
- [ ] Per-tier numbers reconciled against cohort-wide KPIs
- [ ] §1.1 findings escalated immediately, not run through the routine fix loop
