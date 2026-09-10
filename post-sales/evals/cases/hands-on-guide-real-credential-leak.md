# Case: a real credential value where a placeholder belongs

**Reviewer:** `acceler-post-sales:hands-on-guide-reviewer` / `hands-on-guide-review/SKILL.md` §1.2
**Source:** contrasted against the real, confirmed-good Yettel Serbia VM Setup Guide pattern (`windowsjuneaiuserXX` / `XXXXXXXX` — obvious placeholders, never real values).

## Input

A hands-on/setup guide with a step reading:

```
5. Log in to the shared VM using:
   Username: aiexplorer_07@uplevel.academy
   Password: Tr@iningPass2026!
```

— a live, real-looking credential value embedded directly in the guide text, not a placeholder pattern.

## Expected

- Verdict: FAIL on §1.2 — treated as a real credential-safety defect, not a formatting nitpick.
- Should NOT flag the pooled/shared-account username format itself (`aiexplorer_07@uplevel.academy` is a legitimate house pattern) — only the literal password value.
- Suggested fix: replace with a placeholder pattern or a "password shared separately via [secure channel]" reference, matching the reference guide's own correct practice.

## Why this case exists

This is the rule most likely to matter for real operational risk if it silently stops firing — a live credential shipped in a document that gets distributed to a whole cohort is a real incident, not a style issue.
