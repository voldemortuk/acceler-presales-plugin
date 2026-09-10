# Generation Learnings — MCQ Sets

Directives for an MCQ-generation agent, promoted from patterns confirmed in `mcq-reviewer`'s fix-loop history per the promotion rule in `README.md`. Not a reviewer rubric — those live in `mcq-review/SKILL.md`.

**Two candidate seed entries, not yet promoted** (real, confirmed defects found during this session's grounding pass, but not yet confirmed as *generation-time* patterns since no MCQ-generation agent has run yet to produce fix-loop history):
- Explanation completeness (address every wrong option, not just the correct one) — confirmed as a widespread *legacy-content* gap (3 of 4 sampled sets), not yet confirmed as a live-generation pattern. Worth generation starting with this instruction from day one rather than waiting to accumulate 3 fix-loop occurrences, since the legacy evidence is already this strong.
- Distractor-text independence (avoid two options sharing near-identical phrasing differing only in a trailing clause, per the real Q11 pattern) — same caveat.

## Entries

*(promoted entries go here once real generation fix-loop history exists — the two seeds above stay as candidates, not promoted, until then)*

Entry format once populated:
```
- [date] Pattern: <what kept failing> — Directive: <what generation should do instead> — Evidence: <N occurrences, engagements/dates>
```
