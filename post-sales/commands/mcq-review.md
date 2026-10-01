---
description: "POST-SALES / delivery quality gate. Review one MCQ set for item-writing validity (NBME rules) and curriculum/objective alignment — human-gated fix loop, PR-vocabulary verdict. Kept separate from /acceler-post-sales:assignment-review since MCQ defects and rubric defects are different failure modes. For the full session bundle including cross-artifact coherence, use /acceler-post-sales:content-review instead."
argument-hint: "<path to the MCQ set, or 'review last'> + the client/program/day"
---

Review this MCQ set against `skills/mcq-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

**Scope (added 2026-10-01):** when the handoff or the human gives a mode, a scope and a light brief path, read them, pass them to the reviewer, and review against that scope, per `skills/agent-loops/SKILL.md` §4.

1. Read `skills/mcq-review/SKILL.md` in full first — the rubric (§1), mechanics (§2), fix loop (§3), and Memories log (§4).
2. Confirm the session's stated learning objectives are in hand: the approved Lesson Plan / Facts Sheet where they exist, otherwise `Outputs/[Client]/light-brief.md`'s objectives outline, per `skills/agent-loops/SKILL.md` §4. Ask only if none of these exist, never invent them.
3. Launch the `acceler-post-sales:mcq-reviewer` agent per §2, evaluating every item individually — read-only by tool restriction, blind to how the set was generated.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL: propose the fix, wait for explicit human approval, apply via the `acceler-post-sales:content-fixer` agent, re-verify fresh. Never apply without approval; never exceed 2 rounds. The fixer may only touch the item — never the stated objective or this skill's rubric.
6. Report the §5 verdict with evidence for anything not a clean Approve.

## Quality checklist (apply before presenting results)

- [ ] Objectives sourced from the approved Lesson Plan / Facts Sheet, or from the light brief's objectives outline where those don't exist (`agent-loops/SKILL.md` §4), not invented
- [ ] Every item checked individually, not just the set as a whole
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
