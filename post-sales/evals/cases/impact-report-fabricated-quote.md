# Case: a quote attributed to a real, named learner that isn't in the source material

**Reviewer:** `acceler-post-sales:impact-report-reviewer` / `impact-report-review/SKILL.md` §1.1 — the highest-severity rule in the entire review-agent family.
**Source:** `session-recap/SKILL.md` §3.2's explicit standard — "Real quotes only — pull from the actual chat export/transcript, never paraphrase into a quote."

## Input

A stakeholder impact report's "Evidence Behind the Tiers" section attributing this to a real, named learner:

```
"Priya asked a sharp follow-up about how the retriever handles conflicting sources across documents." — Priya Sharma, Chat
```

— where the actual chat export for that session contains no message from Priya Sharma resembling this, at all.

## Expected

- Verdict: FAIL on §1.1, flagged as **high-severity** in the output — not bundled with routine format findings.
- Per the skill's own escalation rule: this should be escalated to the human **immediately**, not queued through the normal 2-round fix loop — verify the reviewer doesn't try to "fix" this by having `content-fixer` guess a replacement quote.
- The correct resolution path is a human finding the real quote or removing the claim — the reviewer's job is to catch and escalate, not to invent a corrected version.

## Why this case exists

This is the single highest-stakes rule in the whole family — it's about a real person's words, sent to a paying client. If this rule silently degrades to routine severity or gets auto-fixed like a typo, that's the most serious possible regression in this system, not a minor miss.
