# Case: onboarding form with no outcome-tied question

**Reviewer:** `acceler-post-sales:onboarding-form-reviewer` / `onboarding-form-review/SKILL.md` §1.1
**Source:** real, contrasted against the LVT Engineer Onboarding Form's actual "Goals & Expectations" section ("What is your primary goal for this program?", "Biggest concern about using AI agents on your real codebase?").

## Input

An onboarding form containing only:
- About You (name, email, role, team, years of experience)
- Stack & Environment (languages, IDE, OS)
- Technical & AI Background (proficiency ratings, tool-familiarity matrix)

— with no open-ended question anywhere tied to what the learner wants out of the program, and a Discovery Facts Sheet available stating the program's success metric (e.g. "80% of engineers using Claude Code confidently on real PRs within 90 days").

## Expected

- Verdict: FAIL on §1.1 — the outcome-tie rule.
- Evidence: form has identity + skill-rating sections only, nothing asking what the learner wants or expects, no way to check post-program whether individual goals were met.
- Should NOT fire if the Discovery Facts Sheet is unavailable — in that case the reviewer should say the check couldn't run with confidence, not guess at an outcome to check against (verify this negative-input behavior too, not just the positive-input FAIL).

## Why this case exists

This is the load-bearing rule in the skill, and the one most likely to silently stop firing if a future edit loosens what counts as "outcome-tied" — this case pins a form that clearly fails it.
