---
description: "Generates one program Closing Ceremony deck by copying a real precedent Closing PPTX and editing only the engagement-specific fields in place, not by building a new deck. Any embedded assessment questions are pulled from the already-generated and reviewed post-test, not written fresh. Hands off to the existing acceler-post-sales:closing-ceremony-reviewer for the actual review pass."
argument-hint: "<client name>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Generate the Closing Ceremony deck for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/closing-ceremony-generation/SKILL.md` in full first. It is the single source for how this deck is built, nothing below overrides it.
2. Load the mandatory inputs per §1: a real precedent Closing deck (this client's own if one exists, otherwise the closest one in `knowledge/engagement-catalog.md`), exported to `.pptx` as the working copy, plus `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If any is missing, handle the gap per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Work through the three tiers in §2: leave Tier 1 slides untouched apart from branding, retailor Tier 2 slides (Key Takeaways must name what this program really covered, from the Lesson Plan), fully swap the Tier 3 fields. Branding per `content-generation/SKILL.md` §1d.
4. If the deck embeds assessment questions, copy them from the reviewed `Outputs/[Client]/mcq/post-test.docx` per §2a, never the pre-test file, never write fresh questions here.
5. Generate the live/external version only, a Dry Run is the same deck rehearsed internally, not a separate artifact.
6. Apply §2b's two creation-time rules: remove the precedent's own leftover duplicate slides, and edit text in place rather than clearing and rebuilding a shape.
7. Edit the PPTX natively, per §4 and `content-generation/SKILL.md` §1e. This deck is not built through `live-session-deck`'s HTML engine. PDF only after a human approves the PPTX.
8. Self-verify per §3.
9. Save to `Outputs/[Client]/closing-ceremony-deck/closing-ceremony-deck.pptx`, per §5.
10. Hand off to `acceler-post-sales:closing-ceremony-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Real precedent deck and both mandatory inputs loaded, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] Tier 1 untouched apart from branding, Tier 2 retailored (Key Takeaways specific to this program's real curriculum), Tier 3 fully swapped
- [ ] Embedded assessment content, if any, copied from the reviewed `mcq/post-test.docx` (not the pre-test file), not authored fresh
- [ ] Live/external deck generated, not a separate Dry Run artifact
- [ ] Native PPTX edited from the precedent, not rebuilt through the HTML deck engine, PDF only after approval
- [ ] Saved to `Outputs/[Client]/closing-ceremony-deck/`
- [ ] Handed to the existing `closing-ceremony-reviewer`, not reviewed inline here
