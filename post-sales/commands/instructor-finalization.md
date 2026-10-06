---
description: "PIPELINE STAGE 4 (post-sales). Builds a day-by-day instructor roster from a finalized Lesson Plan, reusing the existing acceler-presales:instructors query logic and Knowledge Graph directly, one query per day rather than one for the whole engagement. Avoids assigning the same instructor two days running by default."
argument-hint: "<client name>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here. End every run by adding an entry to the client's run notes, per §6d of the same file.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Finalize the instructor roster for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/instructor-finalization/SKILL.md` in full first.
2. Load `Outputs/[Client]/lesson-plan.xlsx`, approved. If it doesn't exist, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess. If it exists but isn't approved yet, say so and don't present the roster as final, a roster matched against a draft plan is itself a draft.
3. For each day-tab, run the same query `acceler-presales:instructors` already runs against that day's Topic/Subtopic columns, post-sales deliverable branch only, `pre_sales_only` names excluded.
4. Assemble the day-by-day roster. Apply the consecutive-day default, don't assign the same instructor two days running unless it's genuinely needed and the instructor agrees, state any override and why.
5. Check real precedent trackers per §3a without conflating roles, and mark every pick `Proposed` per §3b. A ranking is a recommendation, not a booking: only a human's confirmation turns a day into `Confirmed`, written back into the same roster file.
6. Save to `Outputs/[Client]/instructor-roster.md`.

## Quality checklist (apply before presenting results)

- [ ] Ran against an approved Lesson Plan, not a draft
- [ ] One query per day against that day's actual topics, not a single engagement-wide query
- [ ] Deliverable branch used throughout, no `pre_sales_only` names surfaced
- [ ] Consecutive-day default applied, overrides stated explicitly with reason
- [ ] Every day marked `Proposed` unless a human actually confirmed it
- [ ] Saved to `Outputs/[Client]/instructor-roster.md`
