# Content-Review Golden Evals

Regression protection for the review-agent family — does each reviewer still catch the real defects we already know exist, every time a rubric changes.

## Status: running manually, not via `claude plugin eval`

Claude Code has a native plugin eval harness (`claude plugin eval`, `case.yaml` + `graders/*.md`) — checked directly via `claude plugin eval --help` and `claude plugin eval init --bare` in this environment. **It's gated to early access and returned "currently in early access" when invoked here.** Rather than guess at an unverified schema, these cases are written as plain, human-readable markdown — runnable by hand today, and meant to migrate to the native `case.yaml` format the moment eval access opens on this account. Don't build a parallel custom eval runner in the meantime; that's effort that gets thrown away.

## How to run a case manually, today

1. Open the case file. It names the reviewer command/agent, gives the exact input (a real content excerpt or a path to one), and states the expected verdict + finding.
2. Run the named command (`/acceler-post-sales:mcq-review`, etc.) against that input.
3. Compare the actual output against the case's "Expected" section — did the reviewer flag the right rule, at the right severity, without false-positiving on the adjacent negative case where one exists.
4. A mismatch means either the reviewer regressed (fix it) or the case is stale (update it) — don't silently ignore either.

## Why every case here is grounded, not invented

Each case traces to a real document or a real, cited finding surfaced during this plugin's own development (the MCQ docs, Lesson Plan xlsx, program folders surveyed across ~15 Acceler programs). A rubric rule that exists because of a real defect gets a case here confirming the rule actually catches that defect — otherwise the rule is just prose that was never verified to work.

## How this is the "how do we improve over time" mechanism

Three feedback sources keep this system honest as it runs for real, not just at build time:

1. **This eval set** — regression protection. Add a case here every time a real defect slips through in production (a false negative) or a reviewer wrongly flags something legitimate (a false positive) — that's the trigger, not a scheduled review. A growing, real-defect-backed eval set is what "robust" cashes out to, concretely.
2. **Per-skill Memories logs** — each skill's own dismissed-finding log. When the same finding gets dismissed by a human repeatedly across different engagements, that's a signal the *rule itself* is miscalibrated (too aggressive, or checking something that doesn't actually matter), not that every instance is a one-off — promote it into a rubric change, with a case added here confirming the new behavior.
3. **Impact-report data** (PRD Phase 3, not yet flowing) — once `session-recap-review`/`impact-report-review` output accumulates across real deliveries, it's the intended input for recalibrating the Pacing & Difficulty / Engagement Ratio lenses (already seeded in `mcq-review`/`deck-review` as difficulty-label and slide-count-vs-duration checks) against actual outcomes rather than static rules. Not built yet — this repo has no mechanism today to read `session-recap-review` output back into a rubric. Flagging as the real next dependency, not pretending it's wired.

## Cases

| Case | Reviewer | Real source |
|---|---|---|
| `mcq-near-duplicate-distractors.md` | mcq-reviewer | `MCQ - Building Applications with LLMs.docx` Q11 |
| `mcq-zero-explanations.md` | mcq-reviewer | Bosch/Nucleus/e& Post-Course Assessment pattern |
| `mcq-legitimate-multiselect-not-flagged.md` (negative case) | mcq-reviewer | ETS / e& No-Code Pre-Program Assessment |
| `deck-leaked-solution-link.md` | deck-reviewer | Acceler module-review checklist |
| `assignment-undisclosed-starter-split.md` | assignment-reviewer | e& Low Code capstone template |
| `project-compliance-vs-verified-enforcement.md` | project-reviewer | LVT Stale Order Alerts project guide |
| `onboarding-form-missing-outcome-tie.md` | onboarding-form-reviewer | LVT Engineer Onboarding Form |
| `lesson-plan-duration-mismatch.md` | lesson-plan-reviewer | e& Tech Teams Lesson Plan xlsx |
