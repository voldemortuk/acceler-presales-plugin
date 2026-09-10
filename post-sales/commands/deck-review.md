---
description: "POST-SALES / delivery quality gate. Review one session deck for format correctness and curriculum/objective alignment — human-gated fix loop, PR-vocabulary verdict (Approve/Comment/Request Changes). For the full session bundle including cross-artifact coherence, use /acceler-post-sales:content-review instead."
argument-hint: "<path to the deck, or 'review last' to use the deck generated earlier in this session> + the client/program/day"
---

Review this deck against `skills/deck-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/deck-review/SKILL.md` in full first — the rubric (§1), mechanics (§2), fix loop (§3), and Memories log (§4) all live there.
2. Confirm the session's stated learning objectives are in hand (Curriculum KG or the program's Day-by-Day doc). Ask if they can't be found — never invent them.
3. Launch the `acceler-post-sales:deck-reviewer` agent per §2 — read-only by tool restriction, blind to how the deck was generated.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL: propose the fix, wait for explicit human approval, apply via the `acceler-post-sales:content-fixer` agent, re-verify fresh. Never apply a fix without approval; never exceed 2 rounds per finding. The fixer may only touch the deck — never the stated objectives or this skill's rubric.
6. Report the §5 verdict (Approve/Comment/Request Changes) with evidence for anything not a clean Approve.

## Quality checklist (apply before presenting results)

- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
- [ ] Findings cite `live-session-deck/SKILL.md` design tokens, not generic taste
