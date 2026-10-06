---
description: "POST-SALES / delivery quality gate. Session-level content review: orchestrates the six per-type reviews (deck-review, code-demo-review, mcq-review, assignment-review, hands-on-guide-review, project-review), then runs two bundle-level checks across the whole bundle, cross-artifact coherence and audience fit. The Lesson Plan is not part of this bundle, it has its own /acceler-post-sales:lesson-plan-review. Produces a PR-vocabulary tier (Approve/Comment/Request Changes) as a recommendation and a short human sign-off checklist — nothing ships without explicit human sign-off. Use before a full session's content goes to a client/cohort; use the individual per-type commands for fast feedback right after generating just one artifact."
argument-hint: "<paths to the session's artifact bundle — deck/notebook-or-code/MCQs/assignment/hands-on guide/project, whichever are present> + the aggregated onboarding responses and Discovery Facts Sheet if available + client/program/day + risk tier (high-stakes or standard)"
---

**End every run with the run notes (added 2026-10-06):** append an entry to the client's `run-notes.md`, per `skills/content-generation/SKILL.md` §6d: each finding and what happened to it (approved and fixed, dismissed with the person's reason, escalated), and anything the person changed by hand.

Run the full Acceler content-review panel against this session's bundle.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/content-review/SKILL.md` in full first — this orchestrates the six per-type skills and the two bundle-level hats, it doesn't duplicate their rubrics.
2. Confirm the inputs are in hand: which artifacts are present (§2 step 1, fixture data and post-delivery records excluded), the session's stated learning objectives (the approved Lesson Plan / Facts Sheet where they exist, otherwise `Outputs/[Client]/light-brief.md`'s objectives outline, per `skills/agent-loops/SKILL.md` §4, ask only if none of these exist, never invent them), client/program name, and the risk tier (§3 — high-stakes vs standard; ask if not stated, don't assume standard). When the handoff or the human gives a mode, a scope and a light brief path, read them and pass them to every reviewer below, so the bundle is reviewed against that scope.
3. Run each applicable per-type command first (`deck-review`, `code-demo-review`, `mcq-review`, `assignment-review`, `hands-on-guide-review`, `project-review`), not every session has all six — each reaches its own local verdict via its own fix loop before step 4. The Lesson Plan is not part of this bundle, review it with `lesson-plan-review`.
4. Once all applicable per-type reviewer agents have landed, launch both bundle-level hats per §2 step 3, order doesn't matter between them: the `acceler-post-sales:coherence-reviewer` agent (do the artifacts agree with each other, rubric in §1) and the `acceler-post-sales:audience-fit-reviewer` agent (does the bundle fit the real cohort and the program's stated expectations, rubric in `skills/audience-fit-review/SKILL.md`). Each is given the whole bundle and the per-type verdicts, read-only by tool restriction, blind to each hat's internal reasoning. Run audience-fit even when coherence is clean.
5. Walk any FAIL from either bundle-level hat through the same human-gated fix loop as the per-type skills, same guardrail: the fixer may only touch the one artifact the fix applies to, never the stated objectives, the onboarding/discovery data, or any hat's rubric.
6. Assign the final tier per §4 — present it as a recommendation for the human checklist, not an auto-ship decision.
7. Present: a short plain-language walkthrough (§5) → findings summary (what was auto-fixed with approval, what's still flagged and why) → the human sign-off checklist verbatim from §5, exactly 5 items plus the walkthrough.
8. If `--report-only` is passed, run every hat and return findings only — no fix loop, no questions.

## Quality checklist (apply before presenting results)

- [ ] Risk tier set explicitly (§3) — determines human-attention depth, never which hats run
- [ ] All applicable per-type reviews (up to six) ran and reached a local verdict before either bundle-level hat ran
- [ ] Both coherence-reviewer and audience-fit-reviewer ran, not just one
- [ ] Coherence hat only checks cross-artifact consistency — no re-litigating per-type findings
- [ ] Final tier framed as a recommendation, not an auto-ship; nothing ships without explicit human sign-off
- [ ] Human checklist is exactly the 5 items in `content-review/SKILL.md` §5, preceded by the walkthrough, nothing re-asking what a hat already answered
