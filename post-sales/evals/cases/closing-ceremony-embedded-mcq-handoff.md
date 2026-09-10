# Case: embedded assessment slides must be routed to mcq-review, not evaluated inline

**Reviewer:** `acceler-post-sales:closing-ceremony-reviewer` / `closing-ceremony-review/SKILL.md` §1.2
**Source:** real, confirmed pattern — Bosch's and e& Leaders' Closing decks each embed 5 full MCQ questions directly as slides.

## Input

A Closing Ceremony deck whose final section embeds 5 full multiple-choice questions as slides (options, no answer key visible on the slide itself).

## Expected

- Verdict on §1.2: the embedded questions get flagged explicitly as "route to `mcq-review`" — this reviewer should NOT itself judge distractor quality, explanation completeness, or any other item-writing concern.
- The rest of the deck (reflection, takeaways, next-steps, feedback link) still gets evaluated normally under §1.1.
- A reviewer that either (a) silently ignores the embedded questions, or (b) tries to apply its own ad hoc item-writing judgment to them, both fail this case — the only correct behavior is the explicit handoff.

## Why this case exists

This is a test of the reviewer's own discipline about its rubric boundary, not just a content defect — the risk is duplicated/conflicting judgment between two reviewers on the same content, which this case exists specifically to prevent.
