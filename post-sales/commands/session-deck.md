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
2. For a post-sales delivery day, the primary source is the approved `post-sales/Outputs/[Client]/lesson-plan.xlsx` for that day's tab, plus `post-sales/Outputs/[Client]/instructor-roster.md` for that day's instructor. If a pre-sales proposal was generated earlier in this session instead (`/acceler:proposal`), use its Day 1 / Day 2 / … data. Keep HTML and any future PPTX in sync via the same content either way.
3. The deck follows a **4-movement arc**:
   - **Open** — Cover · Instructor intro · "How the next 2.5 hrs run" · Pop into chat warm-up · "Optimise your experience" 3-card rules
   - **Set up** — Phase divider · Setup split (cards + URL/checklist) · 5 numbered VM/tool steps (each = screenshot + 1-line banner) · Ecosystem 5-card grid
   - **Build**: Phase divider · Pattern stages (Basic → Intermediate → Advanced, wayfinding only) · the day's actual concept-teaching content per §3.16, expanding each load-bearing Lesson Plan topic into a real definition, comparison, or worked example, not just restating the topic name · "Pick your use case" 2-card chooser, only where a real either/or choice exists that day · Handoff to lab
   - **Close**: Dark statement slide with kicker + big line + CTA

## Which reference deck to copy

Corrected 2026-09-14: this used to hardcode "e& PPF" as the reference regardless of client, which is wrong for anyone but that exact engagement, and was even wrong the one time it coincidentally matched on client name, "e& PPF" is Yettel/CETIN in Hungary, a different company from e& (UAE), sharing only a name prefix.

**Simplified 2026-09-14, per direct user decision:** `acceler-nucleus-session-deck/` (`live-session-deck/SKILL.md`'s own stated default) is the fixed reference, always. Don't build automatic same-client-detection logic, and don't skip using a real reference file in favor of building fresh from the abstract slide-type spec, that happened on a real run and produced a worse result than either real option. Actually copy the Nucleus file as the base every time this runs, no exceptions inferred on your own. If a different template is genuinely needed for a specific engagement, the human running this says so explicitly and points at it, this command never guesses.

## Design tokens (lock these, same as whichever reference deck was actually chosen above)

```
--bg:        #FAF7F1   /* cream page */
--card:      #FFFFFF
--navy:      #2C3F8E
--navy-deep: #0F1632
--cyan:      #5BC4D2
--cyan-dk:   #2C3F8E
--cyan-soft: #E8F7FA
--ink:       #1A2240
--text2:     #4B5563
--muted:     #6B7280
--border:    #E5E7EB
--font:      -apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Arial, sans-serif
```

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
post-sales/Outputs/[Client]/session-deck/index.html
```

(Corrected 2026-09-14, the path above used to be a hardcoded, machine-specific location from before `post-sales/Outputs/[Client]/` was established as the standard save location for every post-sales artifact, per `content-generation/SKILL.md` §1a.)

If the deck needs screenshots, list the assets the team needs to add (`assets/slide07_1.png` for VM login, etc.) so they know what to drop in.

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
