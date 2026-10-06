---
description: "POST-SESSION learner recap. Generate the day-N-learner-recap HTML page a learner reads after a live delivery day: topics covered, quiz/discussion, cheat sheet, hands-on labs, parked Q&A, resources. For the stakeholder-facing analytics companion, use /acceler-post-sales:session-impact-report instead (renamed 2026-09-18 from session-recap-report)."
argument-hint: "<day number + session content/notes, or a path to the slide deck + chat log>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Generate a learner-facing post-session recap page for this brief.

## Input
```
$ARGUMENTS
```

## How to build

1. Read `skills/session-recap/SKILL.md` in full first, especially **§0** (the data pointers to ask for upfront) and **§2** (learner recap structure).
2. If any of the §0 learner-recap data points are missing from the input (topics covered, whether a quiz ran, parked questions with attribution, whether this is the last day, instructor continuity), ask the user before drafting rather than inventing plausible content. These are evidence inputs (what actually happened in the room), so they sit outside the start-up check's 3 to 5 question cap: that cap is for facts that can reasonably be assumed, and none of these can.
3. If a prior day's recap exists in this session or repo, use it as the exact structural and design-token reference (same CSS, same TOC/nav-fab pattern, same component classes) — don't reinvent the design per day.
4. Follow §4 (prose style) while writing every paragraph, takeaway, and answer explanation.
5. Run the §5 build checklist before presenting the result.
6. Deploy the folder as a sibling under the program's existing Vercel-connected repo (e.g. `dayN-learner-recap/`), matching prior days' naming exactly. The deployed site is what learners open. Always keep the source too: save the same folder to `Outputs/[Client]/session-recap/day-N/` first, so the reviewer and the next person have a stable path to read from, then deploy from it.
7. Hand off to `acceler-post-sales:session-recap-review` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] All §0 data pointers present, or the user was asked rather than content invented
- [ ] Design tokens/components matched to a prior day's recap where one exists
- [ ] §4 prose style followed throughout
- [ ] Saved to `Outputs/[Client]/session-recap/day-N/`, then deployed as a Vercel-connected sibling folder, matching prior-day naming exactly
- [ ] Handed to the existing `session-recap-reviewer`, not reviewed inline here
