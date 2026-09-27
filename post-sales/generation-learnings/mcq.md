# Generation Learnings, MCQ Sets

Directives for an MCQ-generation agent, promoted from patterns confirmed in `mcq-reviewer`'s fix-loop history per the promotion rule in `README.md`. Not a reviewer rubric, those live in `mcq-review/SKILL.md`.

**Two candidate seed entries, not yet promoted** (real, confirmed defects found during this session's grounding pass, but not yet confirmed as *generation-time* patterns since no MCQ-generation agent has run yet to produce fix-loop history):
- Explanation completeness (address every wrong option, not just the correct one). Confirmed as a widespread *legacy-content* gap (3 of 4 sampled sets), not yet confirmed as a live-generation pattern. Worth generation starting with this instruction from day one rather than waiting to accumulate 3 fix-loop occurrences, since the legacy evidence is already this strong.
- Distractor-text independence (avoid two options sharing near-identical phrasing differing only in a trailing clause, per the real Q11 pattern). Same caveat.

## Entries

- [2026-09-24] Pattern: correct answer conspicuously longer/more detailed than distractors. Directive: after writing each item, compare the correct answer's length against the average distractor length; if it's noticeably longer, either rewrite the distractors to match its level of detail or trim the correct answer, never let length alone be a usable signal. Evidence: 5 of 5 generated files (Day 1, Day 2, Day 3, Day 4 in-session quizzes, pre-test), e& TESTRUN, 2026-09-24.
- [2026-09-24] Pattern: correct-answer letter position isn't evenly spread across a set (e.g. D never correct across 6 items, or correct answers only ever A/B, never C/D). Directive: after finalizing a set, tally which letter holds the correct answer across all items; if any letter is heavily over- or under-represented, reorder options on some items to flatten the distribution. Evidence: 4 of 5 generated files (Day 1, Day 3, Day 4, pre-test), e& TESTRUN, 2026-09-24.
- [2026-09-24] Pattern: a word or phrase from the stem reappears in only one option (usually the correct one), letting a learner match text instead of knowing the content. Directive: before finalizing an item, check whether any stem word appears in exactly one option; if so, either remove it from that option or echo it in a distractor too. Evidence: confirmed on Day 1 (Q3, "manually"/"by hand"), e& TESTRUN, 2026-09-24.
- [2026-09-24] Pattern: em-dashes appearing in stems, options, or explanations. Directive: never use em-dashes or en-dashes anywhere in a generated item, use commas or separate sentences instead, and scan the full set for the character before saving; this is a standing house rule, not MCQ-specific, but it keeps recurring here specifically. Evidence: Day 3 and pre-test explicitly flagged (55 instances in pre-test alone), e& TESTRUN, 2026-09-24.
- [2026-09-24] Pattern: distractors that are absolutist or strawman ("always," "never," "completely wrong") rather than a real, plausible misconception, effectively collapsing a 4-option item to fewer real choices. Directive: every distractor must represent something a learner could genuinely believe from a partial or incomplete understanding, not an obviously-false extreme statement. Evidence: Day 2 (Q3) and Day 3 (4 of 5 items), e& TESTRUN, 2026-09-24.

Entry format once populated:
```
- [date] Pattern: <what kept failing>. Directive: <what generation should do instead>. Evidence: <N occurrences, engagements/dates>.
```
