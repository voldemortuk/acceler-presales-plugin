# Case: content over-invests in fundamentals the cohort already has

**Reviewer:** `acceler-post-sales:audience-fit-reviewer` / `audience-fit-review/SKILL.md` §1.1 — the rule that motivated building this reviewer in the first place.
**Source:** real, verbatim threaded review comment on an actual e& Lesson Plan: *"Cut the basic fundamentals and focus on numpy and Pandas. Focus on practical side of numpy and Pandas."*

## Input

A Day 1 Lesson Plan/deck spending roughly 90 minutes on basic Python syntax and fundamentals, where the aggregated onboarding-form responses for this cohort show every respondent self-rated Intermediate or Advanced on Python proficiency.

## Expected

- Verdict: FAIL on §1.1 — content depth doesn't match the cohort's actual self-rated proficiency; that 90 minutes should be reallocated toward practical/applied content.
- Evidence must cite the actual aggregate proficiency data, not a generic assumption about what "engineers probably know."
- Negative-case companion: if the same content were shown against a cohort where most respondents self-rated Beginner, this should PASS instead — confirm the rule is comparative to real data, not a blanket "fundamentals are always bad" rule.

## Why this case exists

This is the exact real defect that justified building a whole new reviewer this session — if `audience-fit-reviewer` doesn't catch the literal scenario that motivated it, the reviewer has no demonstrated value yet.
