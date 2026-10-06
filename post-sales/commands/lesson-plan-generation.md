---
description: "PIPELINE STAGE 3 (post-sales). Generates the internal, minute-level Lesson Plan (Topic/Objective/Subtopic/Time/Flow/Demo/Tools table, one tab per day) from deep research and the proposal's day-by-day section. Self-checks its own timing math, then hands off to the existing acceler-post-sales:lesson-plan-review for the actual review pass."
argument-hint: "<client name, plus a similar past Lesson Plan to reference if you have one>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here. End every run by adding an entry to the client's run notes, per §6d of the same file.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Generate the Lesson Plan for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/lesson-plan-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/deep-research.md` (approved) and the pre-sales proposal's day-by-day section. If either is missing, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Pull in the existing curriculum/topic library where available to fill in rows, and a similar past Lesson Plan if one was pointed to, for formatting and depth reference.
4. Build the table per §2's structure rules, one tab per day, seven columns per row, correct handling of break/lunch/AMA rows, pre/live/post-class segmentation, objectives matched to demo content, demo weight kept light ahead of any dedicated hands-on day.
5. Calibrate row density and demo depth to the audience signal from deep research's Facts Sheet, don't apply one dense template regardless of audience.
6. Self-verify per §3: compute each day's row-duration sum and confirm it matches that day's stated session length. Fix before proceeding if it doesn't.
7. Save to `Outputs/[Client]/lesson-plan.xlsx`.
8. Hand off to `acceler-post-sales:lesson-plan-review` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, confirmed happening on a real run of this exact command, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] Seven-column structure correct, break/lunch/AMA rows handled as exceptions, not gaps
- [ ] Pre-class, live-class, post-class present as distinct rows per module
- [ ] No orphaned objectives, demo content matches or exceeds each row's Bloom's verb
- [ ] Row-duration math self-checked against stated day length before saving
- [ ] Saved to `Outputs/[Client]/lesson-plan.xlsx`
- [ ] Handed to the existing `lesson-plan-review`, not reviewed inline here
