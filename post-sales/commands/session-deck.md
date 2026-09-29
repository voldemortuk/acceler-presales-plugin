---
description: "POST-SALES / delivery deck. Generate an Acceler live SESSION HTML deck — the one the instructor presents while running a hands-on session (VM setup, screenshots, build steps). For the pre-sales pitch deck, use /acceler-presales:proposal-deck instead. Follows the 4-movement arc + 15 slide types."
argument-hint: "<client name + curriculum outline, or 'use last' to use the latest proposal's day data>"
---

Generate a live **session / delivery** HTML deck for this brief — the deck an instructor presents *during* a hands-on session. (For the client-facing **pre-sales pitch** deck, use `/acceler-presales:proposal-deck`.)

## Input
```
$ARGUMENTS
```

## How to build

1. Read `skills/live-session-deck/SKILL.md` for the exact slide-type library, design tokens, JS controller, and quality checklist, including §2.1a (Nucleus's real palette and fonts, the actual default, not §2.1-2.2's Hungary tokens), §3.16 (concept-teaching content) and §3.17 (Nucleus's real component markup, use this instead of writing new HTML from a description), and §5 (reference-deck selection).
2. **Added 2026-09-16: the Build movement's content now comes from a pre-built plan, not decided here.** Check for `post-sales/Outputs/[Client]/slide-content-plan-day-N.md` first (per `skills/slide-content-planning/SKILL.md`). If it exists, use it as the primary source for every Build-movement content slide, render each planned entry into its named component, don't re-decide content or visual treatment here. If it doesn't exist yet, stop and run `/acceler-post-sales:slide-content-planning` first rather than deciding content on the fly in this command, that's exactly the pattern that produced a thin, plain-text-heavy deck on a real test (three attempts, 104 to 114 words/slide against a real 169 benchmark, only 10 of 49 slides using a real component). For everything outside the Build movement (framing, instructor, VM steps), the primary source is still the approved `post-sales/Outputs/[Client]/lesson-plan.xlsx` for that day's tab, plus `post-sales/Outputs/[Client]/instructor-roster.md` for that day's instructor. If a pre-sales proposal was generated earlier in this session instead (`/acceler:proposal`), use its Day 1 / Day 2 / … data. Keep HTML and any future PPTX in sync via the same content either way.
3. The deck follows a **4-movement arc**:
   - **Open** — Cover · Instructor intro · "How the next 2.5 hrs run" · Pop into chat warm-up · "Optimise your experience" 3-card rules
   - **Set up** — Phase divider · Setup split (cards + URL/checklist) · 5 numbered VM/tool steps (each = screenshot + 1-line banner) · Ecosystem 5-card grid
   - **Build**: Phase divider · Pattern stages (Basic → Intermediate → Advanced, wayfinding only) · the content plan's slides, rendered into their named real components, not re-decided here · "Pick your use case" 2-card chooser, only where a real either/or choice exists that day · Handoff to lab
   - **Close**: Dark statement slide with kicker + big line + CTA
4. **Self-check before saving:** confirm every Build-movement slide in the built deck traces to one entry in the content plan, and that it used the plan's named component, not a plain-text substitute. If the deck drifted from the plan, fix the deck, don't just note the drift.

## Which reference deck to copy

Corrected 2026-09-14: this used to hardcode "e& PPF" as the reference regardless of client, which is wrong for anyone but that exact engagement, and was even wrong the one time it coincidentally matched on client name, "e& PPF" is Yettel/CETIN in Hungary, a different company from e& (UAE), sharing only a name prefix.

**Updated 2026-09-29:** the default reference is whatever `live-session-deck/SKILL.md` §2.0 names as its default (currently the e& Low-Code Day 3 replica at `knowledge/deck-reference/eand-lowcode-day3-replica/`, with its real backgrounds, fixed 1280×720 canvas and type scale). Always copy a real reference file as the base, never build fresh from the abstract slide-type spec, that produced a worse result on a real run. If the human hands over or points at a different real deck, that deck is the reference instead, this command never guesses one on its own.

## Which mode this run is in (read before building)

Per `skills/content-generation/SKILL.md` §6b, decide the mode from what the human actually handed over, not from anything they did or didn't say about review:

- **Fresh build:** nothing existing, built from this engagement's own upstream artifacts. Full review.
- **Adapt:** a real deck from another engagement or an external file is the starting point ("use the e& deck as the reference, build one for Ferguson"). Full review, no shortcut, the adapting step itself introduces defects.
- **Tweak:** an existing deck for this same engagement plus specific named changes. Light targeted check only: confirm each change landed and nothing else moved.

State the detected mode in one line at the start of the run so the human can see it.

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

(Corrected 2026-09-16: the day-N subfolder was missing from this line even though `live-session-deck/SKILL.md` §11 already documents it for multi-day programs, a real run built a second, differently-located Day 1 file because of this gap. Always include the day-N subfolder for a multi-day engagement, never save straight into `session-deck/index.html`, that path collides across every day of the same engagement.)

If the deck needs screenshots, list the assets the team needs to add (`assets/slide07_1.png` for VM login, etc.) so they know what to drop in.

## Hand off to review, always, automatically

**Added 2026-09-16: this command never had an explicit handoff step, unlike every other generation command in this pipeline.** Found the gap on a real run, the deck built cleanly but review was never triggered, because nothing in this file said to. Every content-generation command in this pipeline ends by handing off to its matching reviewer as part of the same run, not a separate step someone has to remember to ask for. This command follows the same rule now: once the deck is saved, hand off to `acceler-post-sales:deck-reviewer` for the actual review pass automatically, in the same run, before presenting results. This applies in fresh-build and adapt mode, whether or not the human mentioned review; in tweak mode, run the targeted check instead (see "Which mode this run is in"). This command does not review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before saving)

- [ ] Cover has client branding + 1 H1 with cyan-accent phrase + 3 chips + keyboard hint
- [ ] Instructor intro: photo · 4-5 specs · featured experience tile · 3-4 bsec blocks
- [ ] Every slide has `data-notes` (1-3 sentences, instructor-facing)
- [ ] Phase divider before each major arc transition
- [ ] Ecosystem grid is 5 cards (or rebuild as `c3` / `c4`)
- [ ] Statement/handoff uses `.dark.stmt` + 1 CTA max
- [ ] Print stylesheet hides progress / brand / fs / counter / zone / notes / nav
- [ ] Keyboard nav + touch swipe work
- [ ] Every load-bearing Lesson Plan topic is actually taught per §3.16, not just named as a pattern-stage label
- [ ] Teaching tone matches the reference deck chosen above, not a generic voice
- [ ] Concept slides use real detail mined from deep-research.md and the Lesson Plan's Flow of Examples column per §3.16, not a thinner paraphrase, and reach for a real §3.17 component before falling back to plain text
- [ ] Handed off to `acceler-post-sales:deck-reviewer` automatically in this same run, not left for someone to separately ask for
