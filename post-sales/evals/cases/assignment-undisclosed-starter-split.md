# Case: starter-kit assignment with no stated split

**Reviewer:** `acceler-post-sales:assignment-reviewer` / `assignment-review/SKILL.md` §1.1
**Source:** the rule is grounded in a real reference — `Sentiment_Analysis_Capstone_Template.docx` (e& Low Code capstone) — which explicitly discloses its split ("this gives you ~40% of the work... those enhancements are where the real learning — and the marks — come from") and correctly PASSes this rule. **The Input below is a constructed negative counterpart** (same starter-kit pattern, split undisclosed) — don't run this case against the real reference file expecting a FAIL; it won't produce one, because that file already does this correctly.

## Input

An assignment brief that hands learners a partially-built n8n workflow (or notebook) as a starter, with no text anywhere stating what fraction of the work is pre-built vs. what's being graded — e.g. the brief just says "extend this workflow to handle returns and refunds" with the starter file attached, and no scoping statement.

## Expected

- Verdict: FAIL on §1.1's starter-kit-split rule.
- Evidence: starter artifact is present, but no explicit statement of what's given vs. what's graded.
- Suggested fix: add an explicit line stating the split (e.g. "This starter implements the base intake flow — that's ~40% of the build. Your work: add the returns/refunds branch and error handling, which is what's graded").

## Why this case exists

Confirms the reviewer distinguishes "a starter kit exists" (fine, common, legitimate) from "a starter kit exists with no stated boundary" (the actual defect) — a naive rule could over-flag every starter-kit assignment instead of only the undisclosed ones.
