---
name: mcq-generation-skills-acceler-item-writing
description: "Generates one MCQ set per session, applying NBME item-writing rules and curriculum/objective alignment at creation time, so mcq-reviewer starts from a stronger baseline instead of catching the same widespread legacy defects again. Routes to the existing mcq-reviewer for the actual review pass. Loops with it over time: two candidate directives (explanation completeness, distractor-text independence) are already strong enough from legacy evidence to apply from day one, not wait for three fix-loop occurrences."
metadata:
  type: reference
---

# Acceler MCQ Generation · Post-Sales · SKILL.md (v1)

**What this produces.** One MCQ set for a session, the same artifact `mcq-review/SKILL.md` already reviews per-item. Kept separate from assignment generation, item-writing defects (distractor quality, cueing) are a different failure mode from rubric/answer-key defects, conflating the two dilutes both.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved. Each item must map to a stated Learning Objective from the relevant day's rows, an item testing untaught content is a FAIL in review, don't generate one in the first place.
- `Outputs/[Client]/deep-research.md`, for audience calibration, how technical the distractors and stem language should read.

**Best-effort:**
- An existing question bank or a similar past MCQ set as a formatting and difficulty reference, where deep research's precedent notes point to one.

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

## 3. Self-verify before handoff, starting with what's already strong evidence

Per `content-generation/SKILL.md` §3 and §6a, and the two candidates already seeded in `generation-learnings/mcq.md`, apply these from day one rather than waiting for three fix-loop occurrences to confirm them, the legacy evidence behind both is already stronger than the usual bar:

- **Explanation completeness.** Every question's explanation addresses why each wrong option is wrong, not only why the correct one is right. Three of four sampled legacy sets had zero explanations at all, don't repeat that gap.
- **Distractor-text independence.** No two options sharing near-identical phrasing that differs only in a trailing clause, a test-taker who spots the shared stem can eliminate one without understanding the content, a confirmed real pattern, not a hypothetical.

---

## 4. Where it gets saved

`Outputs/[Client]/mcq.docx`, matching the house format (real reference sets are `.docx`), same per-engagement folder as everything else.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:mcq-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/mcq.md` and `agent-loops` via the `skills:` frontmatter field. Per §6a, this is not a one-off: once a rule fails three or more times across different generated sets, per the promotion rule, it becomes a directive here, and this skill is expected to actually follow it on the next set generated, not just this one.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Every item traced to a stated Learning Objective, no untaught content tested
- [ ] Single-best-answer vs multi-select format confirmed from the stem before writing distractors
- [ ] No compound claims, no implausible distractors, no cueing, no length giveaway
- [ ] Explanation completeness and distractor-text independence applied from this first run, per §3
- [ ] Answer-key format consistent (letter plus text) across every item
- [ ] Saved to `Outputs/[Client]/mcq.docx`
- [ ] Handed to the existing `mcq-reviewer`, no bespoke review invented
