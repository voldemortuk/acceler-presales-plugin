---
description: "POST-DELIVERY quality gate. Review one learner-facing day-N recap for content fidelity, structural correctness, and house prose style — human-gated fix loop, PR-vocabulary verdict. Companion to /acceler-post-sales:impact-report-review."
argument-hint: "<path to the recap HTML, plus the session deck/notes and chat log/transcript for verification> + client/program/day"
---

Review this session recap against `skills/session-recap-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/session-recap-review/SKILL.md` in full first.
2. Launch the `acceler-post-sales:session-recap-reviewer` agent per §2 — verify parked questions against the real chat log/transcript, verify content against the actual session deck/notes.
3. Check every finding against §4 Memories before surfacing it.
4. Walk through §3's fix loop for any FAIL via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds.
5. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] Content traced against the real session deck/notes
- [ ] Parked questions verified against the actual chat log/transcript
- [ ] Last-day branching logic checked explicitly
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
