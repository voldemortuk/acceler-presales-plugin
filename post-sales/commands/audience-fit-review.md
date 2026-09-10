---
description: "POST-SALES / delivery quality gate. Review a session's content bundle for calibration against the actual audience (aggregated onboarding form responses) and program expectations (Discovery Facts Sheet) — depth calibration, example relevance, concern-responsiveness. Human-gated fix loop, PR-vocabulary verdict. Complements /acceler-post-sales:content-review's coherence check; run standalone or as part of the full bundle pass."
argument-hint: "<paths to the session's content bundle, the aggregated onboarding form responses for this cohort, and the Discovery Facts Sheet if available> + client/program/day"
---

Review this session's content bundle against `skills/audience-fit-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/audience-fit-review/SKILL.md` in full first.
2. Confirm what's available: aggregated onboarding responses, Discovery Facts Sheet. Run only the rules those inputs support — say plainly which couldn't run rather than guessing at an audience profile.
3. Launch the `acceler-post-sales:audience-fit-reviewer` agent per §2.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds. The fixer touches only the specific bundle artifact a finding points at.
6. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] Onboarding responses and Discovery Facts Sheet checked for and used, or their absence explicitly flagged
- [ ] Depth-calibration findings backed by real aggregate data, not assumption
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
