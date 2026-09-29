---
description: "Generates one open-ended, rubric-graded assignment for a session, weighted rubric, explicit starter-kit split, and setup/security checks for code-based assignments handled at creation time. Hands off to the existing acceler-post-sales:assignment-reviewer for the actual review pass."
argument-hint: "<client name + which day/session this assignment is for>"
---

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Generate the assignment for this session.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/assignment-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, stop and ask.
3. If that day's code-demo already exists, build on the same tool or technique rather than a disconnected task.
4. Write the assignment against §2's rules, weighted rubric, explicit starter-kit split if one is provided, real scoring criteria for any subjective item, Apply/Analyze/Create Bloom's level.
5. If code-based, run the conditional checks: no real credentials or PII, no license-incompatible code, data files described, prerequisites and execution environment stated, starting references provided even for open-ended tasks.
6. Self-verify per §3: rubric weights sum to 100%, code-based checks run only if actually code-based.
7. Save to `Outputs/[Client]/assignment.docx`, plus a `starter-kit/` subfolder if one exists.
8. Hand off to `acceler-post-sales:assignment-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Weighted rubric present, weights sum to 100%
- [ ] Starter-kit split stated explicitly where applicable
- [ ] Subjective items have real scoring criteria, not tips alone
- [ ] Code-based conditional checks run only when applicable
- [ ] Saved to `Outputs/[Client]/assignment.docx`
- [ ] Handed to the existing `assignment-reviewer`, not reviewed inline here
