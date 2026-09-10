---
description: "POST-SALES / delivery quality gate. Session-level content review: orchestrates deck-review, code-demo-review, mcq-review, assignment-review, then adds cross-artifact coherence across the whole bundle. Produces a PR-vocabulary tier (Approve/Comment/Request Changes) as a recommendation and a short human sign-off checklist — nothing ships without explicit human sign-off. Use before a full session's content goes to a client/cohort; use the individual per-type commands for fast feedback right after generating just one artifact."
argument-hint: "<paths to the session's artifact bundle — deck/notebook-or-code/MCQs/assignment, whichever are present> + client/program/day + risk tier (high-stakes or standard)"
---

Run the full Acceler content-review panel against this session's bundle.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/content-review/SKILL.md` in full first — this orchestrates the other four skills, it doesn't duplicate their rubrics.
2. Confirm §0-equivalent inputs are in hand: which artifacts are present, the session's real stated learning objectives (Curriculum KG / Day-by-Day doc — ask if missing, never invent), client/program name, and the risk tier (§3 — high-stakes vs standard; ask if not stated, don't assume standard).
3. Run each applicable per-type command first (`deck-review`, `code-demo-review`, `mcq-review`, `assignment-review`) — each reaches its own local verdict via its own fix loop before step 4.
4. Launch the `acceler-post-sales:coherence-reviewer` agent (§1-§2) once all applicable per-type reviewer agents have landed — given the whole bundle and the per-type verdicts, read-only by tool restriction, blind to each hat's internal reasoning.
5. Walk any coherence FAIL through the same human-gated fix loop as the per-type skills, same guardrail: the fixer may only touch an artifact, never a stated objective or any hat's rubric.
6. Assign the final tier per §4 — present it as a recommendation for the human checklist, not an auto-ship decision.
7. Present: a short plain-language walkthrough (§5) → findings summary (what was auto-fixed with approval, what's still flagged and why) → the human sign-off checklist verbatim from §5, exactly 5 items plus the walkthrough.
8. If `--report-only` is passed, run every hat and return findings only — no fix loop, no questions.

## Quality checklist (apply before presenting results)

- [ ] Risk tier set explicitly (§3) — determines human-attention depth, never which hats run
- [ ] All applicable per-type reviews ran and reached a local verdict before the coherence hat ran
- [ ] Coherence hat only checks cross-artifact consistency — no re-litigating per-type findings
- [ ] Final tier framed as a recommendation, not an auto-ship; nothing ships without explicit human sign-off
- [ ] Human checklist is exactly the 5 items in `content-review/SKILL.md` §5, preceded by the walkthrough, nothing re-asking what a hat already answered
