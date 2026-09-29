---
description: "Produces a slide-by-slide content plan for one day, before the deck itself gets built. Decides which real component each concept becomes and writes its actual content, so deck generation only has to render an already-decided plan."
argument-hint: "<client name> <day number>"
---

Plan the slide content for this day, before building the deck.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/slide-content-planning/SKILL.md` in full first.
2. Load the mandatory inputs per §1: that day's approved Lesson Plan tab and the approved Deep Research doc. If either is missing or unapproved, stop and ask.
3. Walk that day's Lesson Plan Parts. For each load-bearing concept, per §3, decide the real component (pipeline, pitfall-card, code-block, compare-table, worked-example) and write its actual content now, mined from Deep Research's real examples and the Lesson Plan's Flow of Examples column.
4. Self-verify per §4: word density and real-component ratio against the Nucleus benchmark (~150-170 words/slide, plain text as the minority, not the default).
5. Save to `post-sales/Outputs/[Client]/slide-content-plan-day-N.md`.
6. Hand off to `/acceler-post-sales:session-deck`, which reads this plan as its primary content source.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded and approved, or the run stopped and asked
- [ ] Only Build-movement content planned, not framing slides
- [ ] Every load-bearing concept assigned a real component, plain text only where nothing fits
- [ ] Real content written now, not left as a placeholder
- [ ] Density and component ratio self-checked before saving
- [ ] Saved to `post-sales/Outputs/[Client]/slide-content-plan-day-N.md`
