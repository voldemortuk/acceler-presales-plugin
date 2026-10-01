---
description: "Generates one in-class hands-on build artifact for a session, a notebook, a code file, or a no-code/low-code build guide, whichever that day's lesson plan row calls for. Actually runs code before handoff to capture real output. Hands off to the existing acceler-post-sales:code-demo-reviewer for the actual review pass."
argument-hint: "<client name + which day/session this demo is for>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Generate the hands-on demo for this session.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/demo-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Determine the shape, notebook/code or no-code build guide, from that day's Libraries/Tools column, don't default to notebook.
4. Build against §2's rules for that shape, worked example before independent practice, scaffolding that fades, Bloom's level matched, no real credentials or PII.
5. If notebook or code, actually run it per §3, capture real output, don't claim it works without evidence.
6. Save to `Outputs/[Client]/demo/`.
7. Hand off to `acceler-post-sales:code-demo-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] Correct shape chosen from the Libraries/Tools column, not defaulted
- [ ] Notebook/code actually executed, real output captured
- [ ] No-code steps are concrete and mechanically followable
- [ ] Worked example precedes independent practice, scaffolding fades across the demo
- [ ] No real credentials, PII, or license-incompatible code
- [ ] Saved to `Outputs/[Client]/demo/`
- [ ] Handed to the existing `code-demo-reviewer`, not reviewed inline here
