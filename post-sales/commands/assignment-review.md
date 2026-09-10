---
description: "POST-SALES / delivery quality gate. Review one assignment for rubric/answer-key correctness, curriculum/objective alignment, and data-boundary safety (if code-based) — human-gated fix loop, PR-vocabulary verdict. For the full session bundle including cross-artifact coherence (does this assignment actually reinforce the notebook), use /acceler-post-sales:content-review instead."
argument-hint: "<path to the assignment, or 'review last'> + the client/program/day"
---

Review this assignment against `skills/assignment-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/assignment-review/SKILL.md` in full first — the rubric (§1), mechanics (§2), fix loop (§3), and Memories log (§4).
2. Confirm the session's stated learning objectives are in hand. Ask if missing — never invent them.
3. Launch the `acceler-post-sales:assignment-reviewer` agent per §2 — read-only by tool restriction, blind to how the assignment was generated.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL: propose the fix, wait for explicit human approval, apply via the `acceler-post-sales:content-fixer` agent, re-verify fresh. Never apply without approval; never exceed 2 rounds. The fixer may only touch the artifact — never the stated objective or this skill's rubric.
6. Report the §5 verdict with evidence for anything not a clean Approve. Flag (but don't block on) the local companion-reinforcement heads-up in §1.2 — the authoritative check runs in `content-review`.

## Quality checklist (apply before presenting results)

- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] §1.3 security checks only applied if the assignment is code-based
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
