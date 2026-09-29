---
description: "Generates the standalone tool-access/setup guide for an engagement, covering every tool the lesson plan requires across all days. Never bakes in a real credential value, cross-checks its tool list against the generated demo where one exists. Hands off to the existing acceler-post-sales:hands-on-guide-reviewer for the actual review pass."
argument-hint: "<client name>"
---

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Generate the hands-on/setup guide for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/hands-on-guide-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved, for the full required-tools list across all days) and `Outputs/[Client]/deep-research.md` (for personal vs. pooled account scheme).
3. Write a complete step-by-step section per tool per §2, concrete numbered steps, access URL, support channel included.
4. Never place a real credential value in the text or in any referenced screenshot, use placeholders or reference a separate secure channel instead.
5. Self-verify per §3: cross-check the tool list against `Outputs/[Client]/demo/` where it already exists, and scan explicitly for anything credential-shaped before saving.
6. Save to `Outputs/[Client]/hands-on-guide.docx`.
7. Hand off to `acceler-post-sales:hands-on-guide-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Every required tool has a complete, concrete step-by-step section
- [ ] No real credential value anywhere, explicitly checked
- [ ] Account scheme matches what deep research indicates
- [ ] Tool list cross-checked against the generated demo where it exists
- [ ] Saved to `Outputs/[Client]/hands-on-guide.docx`
- [ ] Handed to the existing `hands-on-guide-reviewer`, not reviewed inline here
