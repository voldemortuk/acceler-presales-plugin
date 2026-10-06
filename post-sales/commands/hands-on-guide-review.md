---
description: "POST-SALES / delivery quality gate. Review one hands-on/setup guide (tool-access doc, e.g. VM & AI Tools SetUp / Pre-Requisites Setup) for completeness, credential safety, and tool/curriculum alignment — human-gated fix loop, PR-vocabulary verdict. Distinct from /acceler-post-sales:deck-review and :code-demo-review. For the full session bundle including cross-artifact coherence, use /acceler-post-sales:content-review instead."
argument-hint: "<path to the hands-on/setup guide, or 'review last'> + the client/program/day + that day's code-demo/deck paths if available (for the tool-alignment check)"
---

**End every run with the run notes (added 2026-10-06):** append an entry to the client's `run-notes.md`, per `skills/content-generation/SKILL.md` §6d: each finding and what happened to it (approved and fixed, dismissed with the person's reason, escalated), and anything the person changed by hand.

Review this hands-on/setup guide against `skills/hands-on-guide-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

**Scope (added 2026-10-01):** when the handoff or the human gives a mode, a scope and a light brief path, read them, pass them to the reviewer, and review against that scope, per `skills/agent-loops/SKILL.md` §4.

1. Read `skills/hands-on-guide-review/SKILL.md` in full first — the rubric (§1), mechanics (§2), fix loop (§3), and Memories log (§4).
2. If that day's code-demo/deck artifacts aren't provided, run everything except §1.3's tool-alignment check and say plainly that it couldn't run — don't guess at what tools the session actually uses.
3. Launch the `acceler-post-sales:hands-on-guide-reviewer` agent per §2 — read-only by tool restriction, blind to how the guide was generated.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL: propose the fix, wait for explicit human approval, apply via the `acceler-post-sales:content-fixer` agent, re-verify fresh. Never apply without approval; never exceed 2 rounds. The fixer may only touch the guide — never the stated objectives or this skill's rubric.
6. Report the §5 verdict with evidence for anything not a clean Approve. Flag (but don't block on) the local tool-list heads-up in §1.3, the authoritative check runs in `content-review`.

## Quality checklist (apply before presenting results)

- [ ] Every listed tool has a matching complete step-by-step section
- [ ] No real credential value anywhere in the guide
- [ ] Tool/curriculum alignment (§1.3) either ran against real day artifacts as a heads-up (not a FAIL), or was explicitly skipped and said so
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
