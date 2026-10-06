---
description: "Generates one capstone/multi-milestone project, milestone/trap-graded or documentation-brief shape, whichever the engagement calls for. Builds the compliance-vs-verified-enforcement distinction in from day one, not just a compliance check. Hands off to the existing acceler-post-sales:project-reviewer for the actual review pass."
argument-hint: "<client name + which program stage this capstone is for>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here. End every run by adding an entry to the client's run notes, per §6d of the same file.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Generate the capstone project for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/project-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Choose the shape, milestone/trap-graded or documentation-brief, from the engagement's signal, not by default.
4. Build against §2's rules for that shape. If milestone/trap-graded, plant traps and write their forced-and-observed enforcement criteria together, per §3, never a compliance-only check.
5. Self-verify per §4: for the milestone/trap shape, actually build the reference solution and run its test suite, confirm it passes. For the documentation-brief shape, confirm a role-relevance mapping exists if the cohort spans multiple functions.
6. Save to `Outputs/[Client]/project/`.
7. Hand off to `acceler-post-sales:project-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] Correct shape chosen from the engagement's signal, not defaulted
- [ ] Every planted trap has a forced-and-observed enforcement criterion, not compliance-only
- [ ] Reference solution built and its test suite actually run, for the milestone/trap shape
- [ ] Role-relevance mapping present for a multi-function cohort
- [ ] No real credentials, PII, or license-incompatible code
- [ ] Saved to `Outputs/[Client]/project/`
- [ ] Handed to the existing `project-reviewer`, not reviewed inline here
