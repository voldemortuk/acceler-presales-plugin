# Case: MCQ set with zero solution explanations

**Reviewer:** `acceler-post-sales:mcq-reviewer` / `mcq-review/SKILL.md` §1.1 (Explanation completeness)
**Source:** real, confirmed pattern across 3 sampled legacy sets — Bosch Masterclass, Nucleus Masterclass, and e& AI Builder Program Post-Course Assessment.

## Input

A 10-item MCQ set in this shape (real, from the e& Post-Course Assessment):

```
Q1. What type of prompting strategy involves providing multiple examples in the prompt
    to guide the LLM?
a) Role-based prompting  b) Chain-of-Thought prompting  c) Instructional prompting
d) Few-shot prompting
Answer: d
```

— repeated for every item, no explanation of why the correct answer is right or why the distractors are wrong, anywhere in the document.

## Expected

- Verdict: FAIL on §1.1's explanation-completeness rule, for every item in the set.
- Should NOT be treated as a surprise/novel finding — the skill explicitly says this pattern is a confirmed, widespread legacy gap. The reviewer's report should reflect that framing (expect volume, don't treat as anomalous).
- Suggested fix per item: an explanation addressing why the correct answer is right AND why each wrong option is wrong, matching the standard set by `MCQ - Building Applications with LLMs.docx`.

## Why this case exists

This is the single most confirmed real gap found in the whole grounding pass (3 of 4 legacy sets sampled had it). If the reviewer doesn't flag bare `Answer: X` with no explanation, the rule most likely to actually matter in practice is silently not working.
