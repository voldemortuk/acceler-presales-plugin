---
description: "POST-SALES / delivery deck. Generate an Acceler live SESSION HTML deck, the one the instructor presents while teaching a day of the program, built on the real e& class-deck template (cover, instructor, agenda, concept slides, demo link-out, quiz, thank you). Works as a fresh build, an adaptation of a real past deck, or a quick tweak, and hands off to the deck reviewer. For the pre-sales pitch deck, use /acceler-presales:proposal-deck instead."
argument-hint: "<client name + curriculum outline, or 'use last' to use the latest proposal's day data>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Generate a live **session / delivery** HTML deck for this brief — the deck an instructor presents *during* a hands-on session. (For the client-facing **pre-sales pitch** deck, use `/acceler-presales:proposal-deck`.)

## Input
```
$ARGUMENTS
```

## How to build

1. Read `skills/live-session-deck/SKILL.md` in full before building: §1 (the real deck shape), §2.0 and §2.3 (the default template's tokens, real background images and fixed 1280×720 canvas), §3 (the slide-type library, including §3.16 concept-teaching content and §3.17's real component markup, use that markup instead of writing new HTML from a description), and §10 (the quality checklist). The skill is the single source for all of these, nothing about structure, colours or components is restated in this command.
2. **Added 2026-09-16: the Build movement's content now comes from a pre-built plan, not decided here.** Check for `post-sales/Outputs/[Client]/slide-content-plan-day-N.md` first (per `skills/slide-content-planning/SKILL.md`). If it exists, use it as the primary source for every Build-movement content slide, render each planned entry into its named component, don't re-decide content or visual treatment here. If it doesn't exist yet in a fresh build, stop and run `/acceler-post-sales:slide-content-planning` first rather than deciding content on the fly in this command (in adapt mode the reference deck's own slides plus the light brief are the content source, and in tweak mode nothing is re-planned, per `content-generation/SKILL.md` §6c), that's exactly the pattern that produced a thin, plain-text-heavy deck on a real test (three attempts, 104 to 114 words/slide against a real 169 benchmark, only 10 of 49 slides using a real component). For everything outside the Build movement (framing, instructor, VM steps), the primary source is still the approved `post-sales/Outputs/[Client]/lesson-plan.xlsx` for that day's tab, plus `post-sales/Outputs/[Client]/instructor-roster.md` for that day's instructor. If a pre-sales proposal was generated earlier in this session instead (`/acceler:proposal`), use its Day 1 / Day 2 / … data. Keep HTML and any future PPTX in sync via the same content either way.
3. Follow the deck shape in `live-session-deck/SKILL.md` §1 (Open, Set up, Build, Close, as rebuilt from the real e& deck), scaled to the scope the human asked for. A deliberately short deck keeps only the parts named in the scope, it doesn't pad out the full arc.
4. **Self-check before saving:** confirm every Build-movement slide in the built deck traces to one entry in the content plan, and that it used the plan's named component, not a plain-text substitute. If the deck drifted from the plan, fix the deck, don't just note the drift.

## Which reference deck to copy

Corrected 2026-09-14: this used to hardcode "e& PPF" as the reference regardless of client, which is wrong for anyone but that exact engagement, and was even wrong the one time it coincidentally matched on client name, "e& PPF" is Yettel/CETIN in Hungary, a different company from e& (UAE), sharing only a name prefix.

**Updated 2026-09-29:** the default reference is whatever `live-session-deck/SKILL.md` §2.0 names as its default (currently the e& Low-Code Day 3 replica at `knowledge/deck-reference/eand-lowcode-day3-replica/`, with its real backgrounds, fixed 1280×720 canvas and type scale). Always copy a real reference file as the base, never build fresh from the abstract slide-type spec, that produced a worse result on a real run. If the human hands over or points at a different real deck, that deck is the reference instead, this command never guesses one on its own.

## Which mode this run is in

Decided by the start-up check at the top of this command (`content-generation/SKILL.md` §6b and §6c), not restated here. What it changes for this command: fresh build and adapt both end in the full reviewer handoff below, tweak ends in a targeted check of exactly what changed.

## Design tokens

Lock the tokens from `live-session-deck/SKILL.md` §2.0 and §2.3 (the same file the reference deck was copied from), don't restate or invent them here.

## Interactive features (free with the same JS controller)

- Keyboard: ← → / space (next) · ↑ ↓ (prev) · Home / End · `.` (speaker notes) · F (fullscreen)
- Touch swipe (50px threshold)
- Bottom nav with dots + count
- Progress bar at top
- Speaker notes via `data-notes` on every slide (always include — instructor-facing tone)
- `?s=N` deep-link to slide N

## Output

Save to:
```
post-sales/Outputs/[Client]/session-deck/day-N/index.html
```

(Corrected 2026-09-16: the day-N subfolder was missing from this line, as `live-session-deck/SKILL.md` §11 also requires for multi-day programs, a real run built a second, differently-located Day 1 file because of this gap. Always include the day-N subfolder for a multi-day engagement, never save straight into `session-deck/index.html`, that path collides across every day of the same engagement.)

If the deck needs screenshots, list the real assets the team still needs to add (the instructor's photo, any real screenshots) and where each goes under `assets/`, so they know what to drop in.

## Hand off to review, always, automatically

**Added 2026-09-16: this command never had an explicit handoff step, unlike every other generation command in this pipeline.** Found the gap on a real run, the deck built cleanly but review was never triggered, because nothing in this file said to. Every content-generation command in this pipeline ends by handing off to its matching reviewer as part of the same run, not a separate step someone has to remember to ask for. This command follows the same rule now: once the deck is saved, hand off to `acceler-post-sales:deck-reviewer` for the actual review pass automatically, in the same run, before presenting results. This applies in fresh-build and adapt mode, whether or not the human mentioned review; in tweak mode, run the targeted check instead. Pass the mode, the scope and the light brief's path with the handoff, per `content-generation/SKILL.md` §6c. This command does not review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before saving)

- [ ] `live-session-deck/SKILL.md` §10's checklist applied in full, it is the single list for template, slide types, navigation and print rules
- [ ] Start-up check run, mode named in one line, light brief written and shown in adapt or standalone runs
- [ ] Every slide has `data-notes` (1-3 sentences, instructor-facing); demo slides carry the learner link on the button and both links in the notes, per §3.20
- [ ] Every Build-movement slide traces to a content-plan entry (fresh build) or to a reference slide plus the light brief (adapt), using a real §3.17 component, not a plain-text substitute
- [ ] Teaching tone matches the reference deck chosen above, not a generic voice
- [ ] Saved under the full plugin path per `content-generation/SKILL.md` §1a, in the day-N subfolder
- [ ] Handed off to `acceler-post-sales:deck-reviewer` automatically in this same run (fresh build and adapt), with mode, scope and light brief passed along; targeted check only in tweak mode
