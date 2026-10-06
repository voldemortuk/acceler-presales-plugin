---
description: "PIPELINE STAGE 2 (post-sales). Synthesizes everything already known about this client, the saved Discovery Facts Sheet, the proposal, onboarding answers, transcripts, emails, similar past work, into one research brief for Lesson Plan and anything else downstream that needs it. Internal synthesis only, not web research."
argument-hint: "<client name, plus paths to any transcripts/emails you want included>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Run deep research for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/deep-research/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/discovery-facts-sheet.md`, the pre-sales proposal, the learner onboarding form. If any is missing, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Pull in whichever best-effort sources are available, transcripts, team-lead discovery form, emails (ask where these live if not already provided in this session), similar past work. State plainly which ones weren't available rather than inventing content to fill the gap.
4. Produce the brief in the five sections from §2: audience and pacing, day-by-day themes, concrete use cases, tools and constraints, precedent notes, plus a gaps section.
5. Save to `Outputs/[Client]/deep-research.md`.
6. Present it for human approval before treating it as ready. No dedicated reviewer agent for this one, per §3, a human read is the gate.

## Quality checklist (apply before presenting results)

- [ ] All three mandatory inputs loaded, or the gap handled per §6c instead of guessing
- [ ] Best-effort sources used where available, gaps named honestly where not
- [ ] Output matches the five-section shape, not a raw dump of source material
- [ ] No em-dashes or AI-sounding prose tells, per `agent-loops` §2a-1, even without a dedicated reviewer
- [ ] Saved to `Outputs/[Client]/deep-research.md`
