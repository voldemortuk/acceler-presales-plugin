---
name: orientation-generation-skills-acceler-logistics-deck
description: "Generates one program Orientation deck, logistics and expectations content, not teaching content, following the confirmed recurring structure across a real 5-program survey. Reuses the existing live-session-deck design tokens and rendering approach rather than a new deck engine, only the content structure here is new. Routes to the existing orientation-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Orientation Generation · Post-Sales · SKILL.md (v1)

**What this produces, and what it deliberately isn't.** One program Orientation deck, the same artifact `orientation-review/SKILL.md` already reviews. This is forward-looking, logistics and expectations content, not teaching content, there's no Bloom's-verb objective to build toward here, don't force a lesson-plan-style structure onto it.

**Reuse the deck engine, not just the reviewer.** The actual slide rendering, design tokens, and build mechanics belong to the slide-deck work already owned separately (`live-session-deck`, `live-session-deck-builder`). This skill generates the Orientation-specific *content and structure*, not a new deck rendering engine, output through the existing deck tokens/engine rather than inventing a parallel one.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/deep-research.md`, for company/program credibility framing and the client's own context.
- `Outputs/[Client]/lesson-plan.xlsx`, approved, for the actual schedule to build the agenda against, and the finalized instructor roster (`Outputs/[Client]/instructor-roster.md`, if it exists by this point) for the instructor introduction section.

**Best-effort:**
- A similar past Orientation deck as a structural reference.
- `Outputs/[Client]/mcq/pre-test.docx`, already generated and reviewed, for the pre-course assessment section (§2), where a pre-test exists for this program.

---

## 2. Rules, reused from orientation-review, not reinvented

Generate against the confirmed recurring structure `orientation-review/SKILL.md` §1.1 checks for, present across every one of five real programs sampled:

- Company/program credibility framing, who's delivering this and why they're credible.
- Instructor/speaker introduction for every instructor who'll appear, bio, career highlights, specializations, pulled from the finalized roster where it exists.
- Program overview: topic, duration, format, target audience, session flow, and **stated final outcomes for participants**, this last part is the section that ties Orientation back to what the client was actually promised, never skip it.
- A genuinely time-blocked schedule, hour-by-hour or day-by-day, not a vague "Day 1 / Day 2" label.
- An "Expectations From Learners" section, focus, participation, and doubt-resolution norms.
- **Pre-course assessment section, generation-side (corrected 2026-09-18, matching Closing Ceremony's already-explicit rule below):** if embedding or linking the pre-test, it comes from `Outputs/[Client]/mcq/pre-test.docx`, already generated and reviewed by `mcq-reviewer`, don't write fresh unreviewed questions directly into this deck. Previously this section only said "a pointer to the pre-course assessment," vague enough that it was never clear which file to actually pull from.

**Not applicable here, don't force it:** Bloom's-verb objective alignment, this deck type has no teaching objective to check against.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3: confirm the schedule section is actually time-blocked (real times, not a placeholder day label) before saving, confirm the "final outcomes" section states something specific to this engagement, not boilerplate carried over from a template, and if embedding assessment content, confirm it was pulled from the already-reviewed `mcq/pre-test.docx`, not authored fresh inside this deck.

---

## 4. Where it gets saved

`Outputs/[Client]/orientation-deck/`, same per-engagement parent folder as everything else, format matching whatever the shared deck engine outputs.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:orientation-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `agent-loops` via the `skills:` frontmatter field. **No `generation-learnings/orientation.md` file exists yet**, deliberately, per `generation-learnings/README.md`'s own note that Orientation is a lower-iteration-volume artifact type (built once per program, not repeatedly authored) and shouldn't get speculative infrastructure before a real repeat pattern shows up. Don't create that file preemptively, add it only once a genuine recurring finding actually appears.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] All six structural sections from §2 present, none skipped
- [ ] Schedule is genuinely time-blocked, not a vague day label
- [ ] Final-outcomes section is specific to this engagement, not boilerplate
- [ ] Embedded assessment content, if any, pulled from the already-reviewed `mcq/pre-test.docx`, not authored fresh
- [ ] Bloom's-verb alignment correctly not applied to this deck type
- [ ] Built through the existing deck engine/tokens, not a new rendering mechanism
- [ ] Saved to `Outputs/[Client]/orientation-deck/`
- [ ] Handed to the existing `orientation-reviewer`, no bespoke review invented
