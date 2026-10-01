# Case (negative): legitimate multi-select item must NOT be flagged as ambiguous

**Reviewer:** `acceler-post-sales:mcq-reviewer` / `mcq-review/SKILL.md` §1.1 (Ambiguous best answer)
**Source:** real, ETS Pre-Program Assessment and e& No-Code Pre-Program Assessment both use multi-select items.

## Input

```
Question: Which of the following are core components of a RAG pipeline? (select all that apply)
Option A: Chunking
Option B: Vector store
Option C: UI framework
Option D: Retriever
Correct: A, B, D
```

## Expected

- Verdict **on the ambiguous-best-answer rule**: PASS, this is a legitimate multi-select item (`Correct: A, B, D`), not an ambiguous single-best-answer item. This case measures that one rule only. The sample is deliberately minimal (no explanation, a bare-letter key, one easy distractor), so a thorough reviewer will also raise those separate item-writing findings. That is correct behaviour and does not fail this case (clarified 2026-10-01 after the first full run, where the reviewer passed the rule and raised exactly those).
- The reviewer must detect the multi-select framing (either from explicit "select all that apply" stem language, or from a `Correct:` field listing more than one letter) **before** applying the ambiguous-best-answer rule, and skip that rule for this item.
- A FAIL here (flagging this as ambiguous) is itself the defect this case exists to catch — a false positive, not a true one.

## Why this case exists

This exact false-positive risk is why the rule was rewritten mid-session — without this negative case, a future rubric tweak could silently reintroduce the false positive and nothing would catch it, since every other case in this suite is a true-positive check.
