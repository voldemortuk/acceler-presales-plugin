---
description: "POST-SALES / delivery quality gate. Review one Lesson Plan for structural completeness, timing-math consistency, Bloom's-verb objective alignment, and cross-artifact tool consistency — human-gated fix loop, PR-vocabulary verdict."
argument-hint: "<path to the Lesson Plan, and that day's code-demo/hands-on-guide if available> + client/program/day"
---

Review this Lesson Plan against `skills/lesson-plan-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/lesson-plan-review/SKILL.md` in full first.
2. Launch the `acceler-post-sales:lesson-plan-reviewer` agent per §2. If that day's code-demo/hands-on-guide artifacts aren't available, run §1.1-1.2 only and say plainly that §1.3 couldn't run.
3. Check every finding against §4 Memories before surfacing it.
4. Walk through §3's fix loop for any FAIL via `acceler-post-sales:content-fixer` — human approval required, max 2 rounds. The fixer may never weaken a stated objective to make an alignment finding disappear.
5. Report the §5 verdict.

## Quality checklist (apply before presenting results)

- [ ] Break/lunch/AMA rows correctly recognized as legitimate
- [ ] Row-duration sums checked against the stated day length
- [ ] Every objective traced to Flow/Demo content
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
