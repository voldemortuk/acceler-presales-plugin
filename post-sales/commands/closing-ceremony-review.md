---
description: "POST-SALES / delivery quality gate. Review one program Closing Ceremony deck for wrap-up completeness, handing off any embedded assessment slides to mcq-review — human-gated fix loop, PR-vocabulary verdict. Distinct from /acceler-post-sales:orientation-review."
argument-hint: "<path to the Closing Ceremony deck (live/external version, not the internal Dry Run rehearsal copy)> + client/program"
---

Review this Closing Ceremony deck against `skills/closing-ceremony-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/closing-ceremony-review/SKILL.md` in full first.
2. Confirm this is the live/external deck, not an internal Dry Run rehearsal copy (which embeds the full assessment for internal use only).
3. Launch the `acceler-post-sales:closing-ceremony-reviewer` agent per §2. Any embedded assessment slides get flagged as "route to mcq-review," not evaluated inline.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds.
6. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] Confirmed live/external deck, not the Dry Run copy
- [ ] Embedded assessment content explicitly routed to mcq-review, not evaluated inline
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
