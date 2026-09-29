---
name: orientation-generation-skills-acceler-logistics-deck
description: "Generates one program Orientation deck by copying a real precedent deck (this client's own real prior one where it exists, otherwise the closest real precedent) and editing only the identified engagement-specific fields in place, native PPTX, not rebuilt through live-session-deck's HTML engine. Grounded 2026-09-24 in the real, actually-delivered e& AI Builder Program (Low Code) Orientation deck, read directly, not assumed. Routes to the existing orientation-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Orientation Generation · Post-Sales · SKILL.md (v2)

**What this produces, and what it deliberately isn't.** One program Orientation deck, the same artifact `orientation-review/SKILL.md` already reviews, forward-looking, logistics and expectations content, not teaching content.

**Rewritten 2026-09-24, real finding.** The previous version of this skill generated Orientation fresh each time through `live-session-deck`'s HTML token/component engine. Reading the real, actually-delivered e& Orientation deck directly (`PowerUp | AI Builder Program (Low Code) for e& - Orientation`) showed that's the wrong model entirely: a real Orientation deck is a native PowerPoint/Slides file, roughly 85% fixed company-template content identical across engagements, with only a specific, identifiable handful of fields that actually change per client. Generating it fresh through an HTML engine built for content-heavy teaching decks produced wrong branding and generic-feeling slides, because it was solving the wrong problem. The fix: copy the real precedent deck, edit only the real swap zones, in place. Per `content-generation/SKILL.md` §1e.

---

## 1. Inputs

**Mandatory:**
- **A real precedent Orientation deck.** For this client, if one already exists (check `post-sales/knowledge/engagement-catalog.md` and this client's real Drive folder first), use it directly, it's the most accurate possible precedent. Otherwise use the closest real precedent per the catalog. Never generate from nothing, a real base deck to edit is not optional. Export it to `.pptx` as the working copy.
- `Outputs/[Client]/lesson-plan.xlsx`, approved, for the real day-by-day curriculum content (§2, tier 2) and the real schedule (duration, dates, time).
- `Outputs/[Client]/deep-research.md`, for program-specific framing where the precedent's own wording needs adapting.

**Best-effort:**
- `Outputs/[Client]/instructor-roster.md`, if finalized by this point, for the instructor introduction slide (§2, tier 3).
- `Outputs/[Client]/mcq/pre-test.docx`, already generated and reviewed, for the embedded assessment question slides (§2, tier 3), where this program embeds them rather than only linking out.

---

## 2. Three-tier structure, not a flat fixed/generate split

**Confirmed 2026-09-24 by reading the real e& deck slide by slide, not sampled.** It's not simply "some slides are fixed, others are generated." Some slides keep the same heading and structure across every engagement but still need their actual content retailored. Three real tiers:

**Tier 1, fully fixed, reused byte-identical from the precedent:** company credibility framing (Who We Are, alumni/instructor-pool stats), founder bios, client testimonials, the "Agentic AI Revolution" context slides, the "What Will Not Be Covered" boilerplate categories, "Expectations From Learners," Communication Channels/WhatsApp instructions, the closing "Plug In & Power Up" founder slide. Don't touch the content of these, only the branding (see below).

**Tier 2, same heading/structure, content retailored to this engagement:** Program Overview (real duration, real dates, real time, delivery mode, prerequisites, all from the Lesson Plan), Curriculum Details (real day-by-day topics, pulled from the Lesson Plan's actual Part/pod structure, not the precedent's own topics), the AI Tools slide (this cohort's real tool list). Same slide shape and heading every time, different real content every time.

**Tier 3, fully swapped:** client/program name wherever it appears (title, footer, badges), the instructor introduction slide (from the finalized roster, flagged Proposed vs. Confirmed per `instructor-finalization`'s own rule if not yet confirmed), the Onboarding Form link, and the actual embedded assessment question slides (pulled verbatim from the already-reviewed `mcq/pre-test.docx`, per §2a below, never authored fresh in the deck).

**Branding follows `content-generation/SKILL.md` §1d, independent of which tier a slide is in.** Acceler logo/branding by default, even on slides copied from a PowerUp-branded precedent, swap it. Only keep PowerUp branding when a human explicitly asks for it on this run.

---

## 2a. Embedded assessment, generation-side

If this deck embeds the pre-course assessment as slides (rather than only linking out), those question slides come from `Outputs/[Client]/mcq/pre-test.docx`, already generated and reviewed by `mcq-reviewer`, copied in verbatim, never authored fresh inside this deck. Per `mcq-generation/SKILL.md` §2a, the real default shape is 10 single-correct MCQs + 2 subjective questions, no multi-select, so expect roughly that many question slides unless a human specified a different count for this engagement's pre-test.

---

## 2b. Two real defects found on the first live run, now creation-time rules

**Added 2026-09-24, confirmed by `orientation-reviewer` and a direct OOXML diff on the e&-TESTRUN run, not a hypothetical:**

- **Dedupe the precedent's own leftover content before saving.** A real precedent deck can carry its own edit history, e.g. two versions of the same instructor-intro slide left in from when the source file itself was updated between cohorts. Copying it wholesale carries that duplication forward. Before saving, scan for slides that are near-duplicates of each other (same section, same layout, different specific names/details) and resolve which one is actually current, per the same finalized roster or Facts Sheet this run is building against, removing the stale one rather than leaving both.
- **Edit text in place, don't clear-and-rebuild the shape.** The first real run's edit method cleared each swap-zone text box and rewrote it, which silently flattened the precedent's real formatting (bullets, paragraph spacing, per-run color) down to uniform plain bold text. Per §1e's own "preserve the source file's real formatting" rule, restated here because the first real attempt at it violated it: edit the existing runs' text content directly (python-pptx run-level `.text` assignment on the existing runs, not `clear()` followed by `add_run()`), so the shape's own formatting survives untouched. Confirm this by diffing the edited shape's XML against the precedent's own, not just eyeballing the rendered slide.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3:
- Confirm every Tier 1 slide's actual content is untouched from the precedent, only branding changed.
- Confirm every Tier 2 slide's content is real and specific to this engagement (real dates, real day-by-day topics from the Lesson Plan), not still showing the precedent's own numbers.
- Confirm every Tier 3 field is genuinely swapped, no leftover precedent client name, no placeholder instructor, no fabricated assessment question.
- Confirm branding matches §1d's default (Acceler) unless a human explicitly asked for PowerUp.

---

## 4. Format and export

Per `content-generation/SKILL.md` §1e: native `.pptx`, edited in place from the real precedent file, source formatting/fonts/theme preserved untouched. **PDF is generated only after a human approves the PPTX**, not on every fix-loop round.

---

## 5. Where it gets saved

`Outputs/[Client]/orientation-deck/orientation-deck.pptx` (and `.pdf` once approved), same per-engagement parent folder as everything else, also pushed to `B2B AI Programs` per `content-generation/SKILL.md` §1b (learner-facing).

---

## 6. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:orientation-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `agent-loops` via the `skills:` frontmatter field. **No `generation-learnings/orientation.md` file exists yet**, deliberately, per `generation-learnings/README.md`'s own note that Orientation is a lower-iteration-volume artifact type, don't pre-build speculative infrastructure before a real repeat pattern shows up.

---

## 7. Checklist
- [ ] A real precedent deck identified and exported to `.pptx`, or the run stopped and asked, never generated from nothing
- [ ] Tier 1 slides (Who We Are, testimonials, founder bios, Agentic AI Revolution, What Will Not Be Covered, Expectations, Communication Channels) copied untouched except branding
- [ ] Tier 2 slides (Program Overview, Curriculum Details, AI Tools) keep the precedent's heading/structure but show this engagement's real content
- [ ] Tier 3 fields (client name, instructor slide, Onboarding Form link, assessment questions) fully swapped, no leftover precedent content
- [ ] Embedded assessment questions, if any, pulled verbatim from the already-reviewed `mcq/pre-test.docx`, not authored fresh
- [ ] No near-duplicate leftover slides carried over from the precedent's own edit history, per §2b
- [ ] Edited text preserves the precedent's real formatting (bullets, spacing, color), edited in place not cleared-and-rebuilt, per §2b
- [ ] Branding matches Acceler default per §1d, unless a human explicitly asked for PowerUp
- [ ] Built by editing the real precedent PPTX in place, not regenerated through `live-session-deck`'s HTML engine
- [ ] PDF only generated after human approval, not every fix-loop round
- [ ] Saved to `Outputs/[Client]/orientation-deck/` locally and to `B2B AI Programs` per §1b
- [ ] Handed to the existing `orientation-reviewer`, no bespoke review invented
