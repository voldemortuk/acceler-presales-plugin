# Case: row durations don't sum to the stated day length

**Reviewer:** `acceler-post-sales:lesson-plan-reviewer` / `lesson-plan-review/SKILL.md` §1.1
**Source:** real gap identified during grounding — nothing in any sampled Lesson Plan previously verified this math, confirmed across the e& Tech Teams xlsx.

## Input

A Lesson Plan day-tab whose row `Estimated Time` values, summed, total 5h 45min, while the program's stated day length (per the proposal/Day-by-Day doc) is a 5-hour day.

## Expected

- Verdict: FAIL on §1.1's row-duration-sum rule.
- Evidence: the specific sum (5h 45min) vs. the stated day length (5h), and which rows account for the overrun.
- Should correctly NOT flag legitimate break/lunch/AMA rows as the source of the mismatch unless they're genuinely where the extra time sits — don't let the fix suggestion just delete a break to make the math work.

## Why this case exists

This is a previously-unchecked gap this session's grounding pass surfaced directly — before this rule existed, nothing verified lesson-plan timing math at all. This case is what proves the new rule actually catches the exact defect it was written for.
