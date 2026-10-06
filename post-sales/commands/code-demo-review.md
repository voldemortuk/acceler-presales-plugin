---
description: "POST-SALES / delivery quality gate. Review one notebook, code lab, or no-code/low-code build guide for technical correctness, worked-example/scaffolding pedagogy, curriculum alignment, and data-boundary safety — human-gated fix loop, PR-vocabulary verdict. Covers all three shapes: notebooks, standalone code files, and no-code/low-code build guides (e.g. a Copilot Studio or Figma Make build). For the full session bundle including cross-artifact coherence, use /acceler-post-sales:content-review instead."
argument-hint: "<path to the notebook, code file(s) or build guide, or 'review last'> + the client/program/day"
---

**End every run with the run notes (added 2026-10-06):** append an entry to the client's `run-notes.md`, per `skills/content-generation/SKILL.md` §6d: each finding and what happened to it (approved and fixed, dismissed with the person's reason, escalated), and anything the person changed by hand.

Review this notebook, code lab or build guide against `skills/code-demo-review/SKILL.md`.

## Input
```
$ARGUMENTS
```

## How to run

**Scope (added 2026-10-01):** when the handoff or the human gives a mode, a scope and a light brief path, read them, pass them to the reviewer, and review against that scope, per `skills/agent-loops/SKILL.md` §4.

1. Read `skills/code-demo-review/SKILL.md` in full first — the rubric (§1), mechanics (§2), fix loop (§3), and Memories log (§4).
2. Confirm the session's stated learning objectives are in hand: the approved Lesson Plan / Facts Sheet where they exist, otherwise `Outputs/[Client]/light-brief.md`'s objectives outline, per `skills/agent-loops/SKILL.md` §4. Ask only if none of these exist, never invent them.
3. Launch the `acceler-post-sales:code-demo-reviewer` agent per §2 — may execute code/notebooks via Bash to verify, but read-only on the artifact itself, blind to how it was generated.
4. Check every finding against §4 Memories before surfacing it.
5. Walk through §3's fix loop for any FAIL: propose the fix, wait for explicit human approval, apply via the `acceler-post-sales:content-fixer` agent, re-verify fresh. Never apply without approval; never exceed 2 rounds. The fixer may only touch the artifact — never the stated objectives or this skill's rubric.
6. Report the §5 verdict with evidence for anything not a clean Approve.

## Quality checklist (apply before presenting results)

- [ ] Objectives sourced from the approved Lesson Plan / Facts Sheet, or from the light brief's objectives outline where those don't exist (`agent-loops/SKILL.md` §4), not invented
- [ ] Technical-correctness check required actual run output as evidence, not a bare claim it works
- [ ] Security/data-boundary rules ran even on a clean-looking notebook
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
