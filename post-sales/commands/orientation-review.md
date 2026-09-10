---
description: "POST-SALES / delivery quality gate. Review one program Orientation deck for logistics-coverage completeness — human-gated fix loop, PR-vocabulary verdict. Distinct from /acceler-post-sales:closing-ceremony-review and from /acceler-post-sales:deck-review (Bloom's-taxonomy content check doesn't apply here)."
argument-hint: "<path to the Orientation deck> + client/program"
---

Review this Orientation deck against `skills/orientation-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/orientation-review/SKILL.md` in full first.
2. Launch the `acceler-post-sales:orientation-reviewer` agent per §2.
3. Check every finding against §4 Memories before surfacing it.
4. Walk through §3's fix loop for any FAIL via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds.
5. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] "Final outcomes for participants" section checked explicitly
- [ ] §1.2 (Bloom's alignment) correctly NOT applied
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
