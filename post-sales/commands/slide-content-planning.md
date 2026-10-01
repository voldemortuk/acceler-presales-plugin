---
description: "Produces a slide-by-slide content plan for one day, before the deck itself gets built. Decides which real component each concept becomes and writes its actual content, so deck generation only has to render an already-decided plan."
argument-hint: "<client name> <day number>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Plan the slide content for this day, before building the deck.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/slide-content-planning/SKILL.md` in full first.
2. Load the mandatory inputs per §1: that day's approved Lesson Plan tab, the approved Deep Research doc, and that day's already generated and reviewed Demo (Demo runs before this stage). That day's reviewed in-session quiz is read too where it exists, for quiz placement per §3a. If a mandatory input is missing or unapproved, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Walk that day's Lesson Plan Parts. For each load-bearing concept, per §3, decide the real component from `live-session-deck/SKILL.md` §3.17 (pipeline, pitfall-card, code-block, stack-items, metrics, query-cards) and write its actual content now, mined from Deep Research's real examples and the Lesson Plan's Flow of Examples column.
4. Self-verify per §4: word density and real-component ratio against the benchmark in §3 (~150-170 words/slide, plain text as the minority, not the default). That number was measured on the Nucleus deck and has not yet been re-measured against the e& default, treat it as a guide, not a hard gate.
5. Save to `post-sales/Outputs/[Client]/slide-content-plan-day-N.md`.
6. Hand off to `/acceler-post-sales:session-deck`, which reads this plan as its primary content source.

## Quality checklist (apply before presenting results)

- [ ] All three mandatory inputs (Lesson Plan tab, Deep Research, that day's Demo) loaded and approved, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] Only Build-movement content planned, not framing slides
- [ ] Every load-bearing concept assigned a real component, plain text only where nothing fits
- [ ] Real content written now, not left as a placeholder
- [ ] Density and component ratio self-checked before saving
- [ ] Saved to `post-sales/Outputs/[Client]/slide-content-plan-day-N.md`
