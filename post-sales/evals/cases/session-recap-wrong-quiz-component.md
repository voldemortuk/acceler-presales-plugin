# Case: scored quiz component used on a day with no real quiz

**Reviewer:** `acceler-post-sales:session-recap-reviewer` / `session-recap-review/SKILL.md` §1.2
**Source:** `session-recap/SKILL.md` §2.4's explicit component rule — `.q` (scored) reserved for a real quiz, `.pk` (discussion) otherwise.

## Input

A day-N learner recap using the `.q` scored-quiz component (options with a marked correct answer and a green checkmark) for a day where, per the actual session notes, no live quiz ran — only an informal discussion question.

## Expected

- Verdict: FAIL on §1.2 — component misuse, not a style nitpick per the skill's explicit framing.
- Suggested fix: replace with the `.pk` single-discussion-question format matching what actually happened that day.
- Confirm the reviewer checked against the actual session notes/deck to know whether a quiz really ran, rather than inferring it from the recap's own claim (which is exactly what's wrong here).

## Why this case exists

This defect is easy to miss because the recap document *itself* claims a quiz happened by using the scored format — a reviewer that trusts the artifact's own framing instead of checking against the real session record would never catch this.
