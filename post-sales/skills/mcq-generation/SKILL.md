---
name: mcq-generation-skills-acceler-item-writing
description: "Generates one MCQ set at a time, one of three distinct types (pre-test, post-test, or a single day's in-session quiz), applying NBME item-writing rules and curriculum/objective alignment at creation time, so mcq-reviewer starts from a stronger baseline instead of catching the same widespread legacy defects again. Routes to the existing mcq-reviewer for the actual review pass, once per generated file, not once per engagement. Loops with it over time: two candidate directives (explanation completeness, distractor-text independence) are already strong enough from legacy evidence to apply from day one, not wait for three fix-loop occurrences."
metadata:
  type: reference
---

# Acceler MCQ Generation · Post-Sales · SKILL.md (v1)

**What this produces.** One MCQ set per run, the same kind of artifact `mcq-review/SKILL.md` already reviews per-item, but this skill produces **three genuinely different sets**, not one shared file:

- **Pre-test** — Orientation's pre-course assessment. Foundational, checks whether basic concepts landed.
- **Post-test** — Closing Ceremony's post-course assessment. Harder, scenario/applied, calibrated to sit above the pre-test's difficulty, per `mcq-review/SKILL.md` §1.1's own difficulty-calibration rule.
- **In-session quiz** — one per day, scoped only to that day's own content, lighter than either of the above. **Runs before that day's `slide-content-planning`, not after** (`content-generation/SKILL.md` §1), since the plan needs the real, reviewed questions to place them correctly, per `slide-content-planning/SKILL.md` §3a. Deck generation itself never writes or fetches quiz questions, it only renders what the plan already decided.

**Corrected 2026-09-18, real finding from checking actual delivered assessments (e& Orientation and Closing Ceremony decks):** this skill previously produced one flat `mcq.docx`, with Closing Ceremony pulling its post-test from that same file. That's wrong, real pre- and post-tests are different questions at different difficulty levels, not the same instrument reused. Generate each type as its own run, its own file, its own review pass.

Kept separate from assignment generation, item-writing defects (distractor quality, cueing) are a different failure mode from rubric/answer-key defects, conflating the two dilutes both.

---

## 1. Inputs

**Mandatory, every type:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved. Each item must map to a stated Learning Objective from the relevant day's rows, an item testing untaught content is a FAIL in review, don't generate one in the first place. For an in-session quiz, only that specific day's rows are in scope, not the whole engagement.
- `Outputs/[Client]/deep-research.md`, for audience calibration, how technical the distractors and stem language should read.

**Best-effort, every type:**
- An existing question bank or a similar past MCQ set as a formatting and difficulty reference, where deep research's precedent notes point to one.

**Additional, post-test only:**
- `Outputs/[Client]/mcq/pre-test.docx`, already generated and reviewed. Read it to avoid repeating its questions and to confirm the new set is genuinely harder, not just longer, this is what makes the calibration real instead of asserted.

---

## 2. Item-writing rules, reused from mcq-review, not reinvented

Generate against the same rules `mcq-review/SKILL.md` §1.1 checks for, stated here only as creation-time directives:

- No implausible distractors, no malformed negative stems ("EXCEPT"/"NOT" without emphasis), no grammatical cueing, no length/specificity giveaway (the correct answer conspicuously longer or more detailed).
- Confirm single-best-answer versus multi-select from the stem's own framing before writing distractors, don't default to single-best-answer.
- No compound claims disguised as one item (a Yes/No item bundling multiple independent claims), this applies to Yes/No-statement items exactly as much as standard 4-option ones.
- Every wrong option needs a real, plausible reason someone might pick it, not a throwaway.
- Answer-key format: option letter plus its text (`B) Enhancing content filtering systems`), not a bare-text line with no letter.
- Where the house convention tags each item with its sub-topic, include the tag, it's what the alignment check reads directly instead of inferring the mapping.
- Every item's Bloom's level matches or exceeds its mapped objective's stated level.

---

## 2a. Real shape per type, grounded in an actual delivered e& assessment

*Confirmed 2026-09-18 by reading the real Orientation and Closing Ceremony decks directly, not assumed:*

- **Pre-test**: 6 single-correct MCQs + 4 multi-correct MCQs + 2 short subjective questions. Foundational, e.g. "why did this AI output not match the ask" rather than debugging a live system.
- **Post-test**: 10 single-correct MCQs + 2 short subjective questions. Genuinely harder, real examples from the reference set include debugging a broken live automation, distinguishing an agent from a workflow, and naming the specific Responsible-AI principle a scenario violates.
- **In-session quiz**: shorter still, scoped to a single day, lighter-weight knowledge check rather than a formal assessment. Match whatever quiz-slide component the session deck already uses if one exists, don't invent a new question format for it.

These counts are a reference, not a rigid template, calibrate to the actual engagement length and content, but the *shape* (multi-item-type mix, pre-test easier than post-test) is a real, confirmed pattern, not a guess.

---

## 3. Self-verify before handoff, starting with what's already strong evidence

Per `content-generation/SKILL.md` §3 and §6a, and the two candidates already seeded in `generation-learnings/mcq.md`, apply these from day one rather than waiting for three fix-loop occurrences to confirm them, the legacy evidence behind both is already stronger than the usual bar:

- **Explanation completeness.** Every question's explanation addresses why each wrong option is wrong, not only why the correct one is right. Three of four sampled legacy sets had zero explanations at all, don't repeat that gap.
- **Distractor-text independence.** No two options sharing near-identical phrasing that differs only in a trailing clause, a test-taker who spots the shared stem can eliminate one without understanding the content, a confirmed real pattern, not a hypothetical.

---

## 4. Where it gets saved

`Outputs/[Client]/mcq/`, matching the house format (real reference sets are `.docx`), one file per type:
- `Outputs/[Client]/mcq/pre-test.docx`
- `Outputs/[Client]/mcq/post-test.docx`
- `Outputs/[Client]/mcq/day-N-in-session.docx` (one per day, `N` = the actual day number for this engagement)

Also pushed to the `PostSalesPluginOutput` Drive folder per `content-generation/SKILL.md` §1b, same per-type filenames, mirroring this local structure. This is the working/authoring copy, not the delivered instrument, the real test learners take is a form (Orientation and Closing Ceremony hand off to that separately); this file exists so the questions get authored and reviewed properly before either deck embeds them.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:mcq-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/mcq.md` and `agent-loops` via the `skills:` frontmatter field. Per §6a, this is not a one-off: once a rule fails three or more times across different generated sets, per the promotion rule, it becomes a directive here, and this skill is expected to actually follow it on the next set generated, not just this one.

---

## 6. Checklist
- [ ] Type confirmed before starting (pre-test / post-test / day-N in-session), not assumed
- [ ] Both mandatory inputs loaded, or the run stopped and asked; pre-test read first if generating post-test
- [ ] Every item traced to a stated Learning Objective, no untaught content tested; in-session scoped to that one day only
- [ ] Single-best-answer vs multi-select format confirmed from the stem before writing distractors
- [ ] No compound claims, no implausible distractors, no cueing, no length giveaway
- [ ] Explanation completeness and distractor-text independence applied from this first run, per §3
- [ ] Answer-key format consistent (letter plus text) across every item
- [ ] Post-test genuinely harder than the pre-test, not just longer, per §2a
- [ ] Saved to `Outputs/[Client]/mcq/<type>.docx` locally and to `PostSalesPluginOutput` per §4
- [ ] Handed to the existing `mcq-reviewer` for this specific file, no bespoke review invented
