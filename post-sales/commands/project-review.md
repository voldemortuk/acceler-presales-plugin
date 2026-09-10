---
description: "POST-SALES / delivery quality gate. Review one capstone/multi-milestone project (facilitator + learner guide, planted-bug grading, reference-solution answer key) for milestone completeness, grading-signal validity, curriculum alignment, and data-boundary safety — human-gated fix loop, PR-vocabulary verdict. Distinct from /acceler-post-sales:assignment-review. For the full session bundle including cross-artifact coherence, use /acceler-post-sales:content-review instead."
argument-hint: "<path to the project guide + reference solution if available, or 'review last'> + the client/program/day"
---

Review this project against `skills/project-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/project-review/SKILL.md` in full first — the rubric (§1), mechanics (§2), fix loop (§3), and Memories log (§4).
2. Confirm the session's stated learning objectives are in hand. Ask if missing — never invent them.
3. Launch the `acceler-post-sales:project-reviewer` agent per §2 — may run the reference solution's test suite via Bash to verify it, but read-only on the guide itself, blind to how the project was generated. If a reference solution wasn't provided, say so explicitly and note §1.1's reference-solution check couldn't run — don't assume one exists.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL: propose the fix, wait for explicit human approval, apply via the `acceler-post-sales:content-fixer` agent, re-verify fresh. Never apply without approval; never exceed 2 rounds. The fixer may only touch the project guide/reference solution — never the stated objectives or this skill's rubric.
6. Report the §5 verdict with evidence for anything not a clean Approve — give §1.2 (grading-signal validity) special attention in the report, it's the rule most likely to be silently skipped.

## Quality checklist (apply before presenting results)

- [ ] Reference solution checked if provided, or its absence explicitly flagged
- [ ] Every planted trap/constraint matched against an explicit grading criterion
- [ ] §1.2's compliance-vs-verified-enforcement distinction was actually applied
- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
