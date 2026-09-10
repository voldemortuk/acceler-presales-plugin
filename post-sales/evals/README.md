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

See `agent-loops/SKILL.md` §9 for the full two-sided design (reviewer-side and generation-side). Short version: this eval set is one of three reviewer-side feedback sources (alongside per-skill Memories logs and, eventually, impact-report data) — grows on every real false negative/positive, not on a schedule.

## Coverage — every agent in the family has at least one case

18 cases, 16 of 16 agents covered (`content-fixer`'s core guardrail gets its own dedicated case — arguably the single highest-stakes behavior in the whole system). `mcq-reviewer` and `impact-report-reviewer` get extra cases where the real-world risk is highest (item-writing defects are numerous and varied; a fabricated quote about a real person is the single highest-severity failure mode anywhere in this family).

| Case | Agent | Real source |
|---|---|---|
| `mcq-near-duplicate-distractors.md` | mcq-reviewer | `MCQ - Building Applications with LLMs.docx` Q11 |
| `mcq-zero-explanations.md` | mcq-reviewer | Bosch/Nucleus/e& Post-Course Assessment pattern |
| `mcq-legitimate-multiselect-not-flagged.md` (negative case) | mcq-reviewer | ETS / e& No-Code Pre-Program Assessment |
| `deck-leaked-solution-link.md` | deck-reviewer | Acceler module-review checklist |
| `code-demo-no-run-evidence.md` | code-demo-reviewer | skill's own explicit run-evidence rule |
| `assignment-undisclosed-starter-split.md` | assignment-reviewer | e& Low Code capstone template |
| `hands-on-guide-real-credential-leak.md` | hands-on-guide-reviewer | Yettel Serbia VM Setup Guide (contrast case) |
| `project-compliance-vs-verified-enforcement.md` | project-reviewer | LVT Stale Order Alerts project guide |
| `lesson-plan-duration-mismatch.md` | lesson-plan-reviewer | e& Tech Teams Lesson Plan xlsx |
| `discovery-thin-brief-missing-success-metric.md` | discovery-fit-reviewer | skill's own explicit Facts Sheet gap rule |
| `onboarding-form-missing-outcome-tie.md` | onboarding-form-reviewer | LVT Engineer Onboarding Form |
| `orientation-missing-outcomes-section.md` | orientation-reviewer | Bosch Masterclass Orientation (5-program survey) |
| `closing-ceremony-embedded-mcq-handoff.md` | closing-ceremony-reviewer | Bosch / e& Leaders Closing decks |
| `session-recap-wrong-quiz-component.md` | session-recap-reviewer | `session-recap/SKILL.md` §2.4 component rule |
| `impact-report-fabricated-quote.md` | impact-report-reviewer | `session-recap/SKILL.md` §3.2 real-quotes-only standard |
| `coherence-tool-list-drift.md` | coherence-reviewer | confirmed cross-artifact drift class |
| `audience-fit-fundamentals-mismatch.md` | audience-fit-reviewer | real threaded comment on an actual e& Lesson Plan |
| `content-fixer-refuses-to-weaken-objective.md` | content-fixer | `agent-loops/SKILL.md` §3 guardrail |
