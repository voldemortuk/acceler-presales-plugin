# Case: near-duplicate distractor text

**Reviewer:** `acceler-post-sales:mcq-reviewer` / `mcq-review/SKILL.md` §1.1 (Length/specificity giveaway)
**Source:** real, `MCQ - Building Applications with LLMs.docx`, Question 11.

## Input

```
Question-11
What is the characteristic of a zero-shot prompt in the context of language models?
Option A: The prompt will contain unrelated examples
Option B: The prompt contains instructions on how to perform the task but it does not
          contain any examples on how to perform the task. It works only if the model
          has seen the same exact prompt during training
Option C: The prompt contains instructions on how to perform the task but it does not
          contain any examples on how to perform the task. It works well for simple tasks
Option D: The prompt contains instructions on how to perform the task in addition to
          some optional examples on how to perform the task
Correct: C
```

## Expected

- Verdict: FAIL on §1.1's length/specificity-giveaway rule.
- Evidence should name Options B and C specifically — both share the identical opening two sentences, differing only in the trailing clause. A test-taker can eliminate one via text-matching alone without understanding the content.
- Suggested fix should rewrite B (or C) to remove the shared boilerplate, not just note the problem.

## Why this case exists

This is the exact real instance that grounded the length/specificity-giveaway rule when `mcq-review` was built — if the reviewer stops catching this, the rule's worked example in the skill file is no longer actually verified, just asserted.
