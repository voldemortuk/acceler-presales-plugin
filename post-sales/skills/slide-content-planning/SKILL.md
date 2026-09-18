---
name: slide-content-planning-skills-acceler-post-sales-pre-deck-content-brief
description: "Produces a slide-by-slide content plan for one day, before the actual deck gets built. For each load-bearing Lesson Plan concept, decides which real visual component it becomes and writes the actual content for it, mined from Deep Research and the Lesson Plan, not a placeholder. Deck generation then becomes mechanical, take each plan entry, render it into its matching real component, rather than making content and visual-design decisions in the same pass as HTML assembly. This loops with deck-reviewer over time too, once real fix-loop history accumulates against generated decks, patterns that recur get folded back into this skill's own guidance, not just fixed one deck at a time."
metadata:
  type: reference
---

# Acceler Slide Content Planning · Post-Sales · SKILL.md (v1)

**Why this exists.** Found on a real test run (e& AI Builder Low Code, Day 1, 2026-09-14 through 16): three separate attempts to fix a thin, plain-text-heavy deck through instructions alone (a density target, a reference file, explicit prompts) barely moved the numbers, 104 to 110 to 114 words per slide against Nucleus's real 169, and only 10 of 49 slides ever used a real visual component. The facts were always correct, pulled honestly from Deep Research and the Lesson Plan, but the visual-design decision (which component fits this concept) and the content-depth decision (how much real detail to include) kept losing out to the mechanical work of assembling an actual HTML deck, tokens, JS, framing slides, all in the same pass. Splitting the two jobs is the fix: decide what to teach and how to show it here, first, then let deck generation do the much more mechanical job of rendering an already-decided plan.

---

## 1. Inputs

**Mandatory:**
- `post-sales/Outputs/[Client]/lesson-plan.xlsx`, approved, that day's tab.
- `post-sales/Outputs/[Client]/deep-research.md`, approved.
- `post-sales/Outputs/[Client]/demo/` (or wherever that day's Demo landed), already generated and reviewed. **Added 2026-09-18: Demo runs before this stage, not after.** This plan can't build an accurate story around a demo it hasn't actually seen, the real engagement flow is pain point, then concepts, then the live demo, then extend, guessing at what the demo does produces exactly the thin, generic content this whole stage exists to prevent.

**Best-effort:**
- A real precedent deck for this client or a similar engagement, per `post-sales/knowledge/engagement-catalog.md`, for a sense of real pacing and depth, not for copying its specific content.
- `post-sales/Outputs/[Client]/mcq/day-N-in-session.docx` (this day's number), if a quiz was scoped for this day. **Added 2026-09-18:** if one exists, this plan reads the real questions and decides where they land in the concept flow, per §3 below, deck generation then just renders what's already decided rather than writing quiz questions itself at render time.

---

## 2. Scope, deliberately narrow

This plans the **Build movement's content slides only**, per `live-session-deck/SKILL.md` §1, the actual concept-teaching. It does not plan the Open movement (cover, instructor intro, warm-up, house rules) or the Set-up movement (VM steps, ecosystem grid) or the Close movement (statement/handoff), those are fixed, template-driven framing shapes with no content-vs-visual-component decision to make, deck generation still builds them directly from the skill's §3.1-3.15 library, same as always.

---

## 3. For each load-bearing concept, decide two things

Walk the day's Lesson Plan Part by Part. For each one that's load-bearing enough to need real teaching (per `live-session-deck/SKILL.md` §3.16's own judgment, a menu, not a checklist, weight by how central the concept is):

1. **Which real component it becomes**, from `live-session-deck/SKILL.md` §3.17: a sequence or architecture flow is a `pipeline`; a common mistake worth naming is a `pitfall-card`; a real runnable snippet is a `code-block`; a tradeoff between two real options is a `compare-table`; a concept that needs showing before-and-after is the `worked-example` pattern. If nothing real fits, that's the one legitimate case for plain text, not the default.
2. **The actual content**, written now, not a placeholder. Pull the real specifics: the real example from Deep Research's precedent notes and concrete use cases, the real detail from the Lesson Plan's Flow of Examples & Topics column, this cohort's real tools and real stated concerns. Write the real definition sentence, the real comparison rows, the real before/after prompt text, here, so deck generation only has to place it, not invent or paraphrase it down.

**Target density, carried over from the real measured gap:** aim for content close to Nucleus's real benchmark, roughly 150-170 words per planned slide, and don't let more than a small minority of Build-movement slides end up as plain text. Both numbers are self-checkable, per §4, don't leave them to hope.

---

## 3a. Placing that day's quiz, if one exists

**Added 2026-09-18.** If `mcq/day-N-in-session.docx` exists for this day (§1), place its actual, already-reviewed questions into the plan directly, per `live-session-deck/SKILL.md` §3.16's quiz-slide pair, right after the concept it tests, same place a quiz naturally sits in the real repeating Build-movement block. Copy the real question and options in, don't summarize or rewrite them, that file already went through `mcq-reviewer`'s item-writing check, rewriting it here risks reintroducing exactly the defects that review caught. If no in-session quiz was generated for this day, that's fine, it's optional per the live-session-deck menu, don't invent one here just to fill the slot.

---

## 4. Self-verify before handoff

Per `content-generation/SKILL.md` §3: before saving, count the words planned per slide and the real-component ratio across the plan. If most entries are thin or plain-text, that's the same failure mode already found once, go back and mine more real detail rather than shipping a plan that will produce another thin deck.

---

## 5. Where it gets saved

`post-sales/Outputs/[Client]/slide-content-plan-day-N.md`, same per-engagement folder as everything else, one file per day since content is day-specific.

---

## 6. Handoff to deck generation, and this loops over time

`/acceler-post-sales:session-deck` reads this plan as its primary content source for the Build movement, per `live-session-deck/SKILL.md` and `commands/session-deck.md`, both updated to require it. Deck generation's job becomes mechanical for content slides, take each plan entry, render it into its named component's real markup, apply tokens. It must self-check that the built deck actually matches the plan slide-by-slide before presenting, not just build something plausible. `acceler-post-sales:deck-reviewer` also checks deck-vs-plan alignment now, per its own updated §1.1. This isn't a one-time fix, if the same gap between plan and built deck keeps recurring across different engagements, that pattern belongs back in this skill's own guidance, the same connection contract every other generation skill in this pipeline already follows.

---

## 7. Checklist
- [ ] All mandatory inputs loaded and approved, including that day's reviewed Demo, or the run stopped and asked
- [ ] Only Build-movement content planned, framing slides left to deck generation's existing library
- [ ] Every load-bearing concept assigned a real component per §3.17, plain text only where nothing real fits
- [ ] Content written now, real specifics from Deep Research and the Lesson Plan, not a placeholder for deck generation to fill in later
- [ ] If this day has a reviewed in-session quiz, its real questions are placed per §3a, not rewritten
- [ ] Word density and component ratio self-checked against the real Nucleus benchmark before saving
- [ ] Saved to `post-sales/Outputs/[Client]/slide-content-plan-day-N.md` locally, and to `PostSalesPluginOutput` per `content-generation/SKILL.md` §1b
