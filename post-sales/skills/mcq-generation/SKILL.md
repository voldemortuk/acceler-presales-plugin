---
name: mcq-generation-skills-acceler-item-writing
description: "Generates one MCQ set at a time, one of three distinct types (pre-test, post-test, or a single day's in-session quiz), applying NBME item-writing rules and curriculum/objective alignment at creation time, so mcq-reviewer starts from a stronger baseline instead of catching the same widespread legacy defects again. Routes to the existing mcq-reviewer for the actual review pass, once per generated file, not once per engagement. Loops with it over time: two candidate directives (explanation completeness, distractor-text independence) are already strong enough from legacy evidence to apply from day one, not wait for three fix-loop occurrences."
metadata:
  type: reference
---

# Acceler MCQ Generation · Post-Sales · SKILL.md (v1)

**How this skill is started (added 2026-10-01):** every run begins with the start-up check in `content-generation/SKILL.md` §6c (name the mode per §6b, check what already exists for this client, ask at most 3 to 5 questions, write a light brief in adapt or standalone runs). Where this file says an input is mandatory or says to stop when one is missing, that describes a fresh build inside the full pipeline; in adapt or standalone runs the gap is handled the §6c way instead. Before building, also read `agent-loops/SKILL.md` (§2a, §2a-1, §2b) and this artifact type's `generation-learnings/` file where one exists, they are not loaded automatically.

**What this produces.** One MCQ set per run, the same kind of artifact `mcq-review/SKILL.md` already reviews per-item, but this skill produces **three genuinely different sets**, not one shared file:

- **Pre-test** — Orientation's pre-course assessment. **Corrected 2026-09-24, real finding:** this is the harder, diagnostic one, scenario/applied questions calibrating the learner's real baseline before anything's been taught, per §2a below.
- **Post-test** — Closing Ceremony's post-course assessment. **Corrected 2026-09-24:** this is the easier, more foundational one, checking whether the specific vocabulary and concepts just taught actually landed, per §2a below. Checked `mcq-review/SKILL.md` §1.1 for the same assumption, it already just says "verify calibration is sensible," doesn't assert post is always harder, no correction needed there.
- **In-session quiz** — one per day, scoped only to that day's own content, lighter than either of the above. **Runs before that day's `slide-content-planning`, not after** (`content-generation/SKILL.md` §1), since the plan needs the real, reviewed questions to place them correctly, per `slide-content-planning/SKILL.md` §3a. Deck generation itself never writes or fetches quiz questions, it only renders what the plan already decided.

**Corrected 2026-09-18, real finding from checking actual delivered assessments (e& Orientation and Closing Ceremony decks):** this skill previously produced one flat `mcq.docx`, with Closing Ceremony pulling its post-test from that same file. That's wrong, real pre- and post-tests are different questions at different difficulty levels, not the same instrument reused. Generate each type as its own run, its own file, its own review pass.

Kept separate from assignment generation, item-writing defects (distractor quality, cueing) are a different failure mode from rubric/answer-key defects, conflating the two dilutes both.

---

## 1. Inputs

**Mandatory, every type:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved. Each item must map to a stated Learning Objective from the relevant day's rows, an item testing untaught content is a FAIL in review, don't generate one in the first place. For an in-session quiz, only that specific day's rows are in scope, not the whole engagement. **Added 2026-09-18, per `content-generation/SKILL.md` §2: the single most load-bearing granular fact here is whether the relevant day's Learning Objective column is actually filled in**, not just present as a column. A blank or vague objective for a row this set needs to test means there's nothing real to map to, don't stop, but flag it plainly and either skip that row or state the objective was inferred, never silently invent one and present it as stated.
- `Outputs/[Client]/deep-research.md`, for audience calibration, how technical the distractors and stem language should read.

**Best-effort, every type:**
- An existing question bank or a similar past MCQ set as a formatting and difficulty reference, where deep research's precedent notes point to one.

**Additional, post-test only:**
- `Outputs/[Client]/mcq/pre-test.docx`, already generated and reviewed. Read it to avoid repeating its questions and to confirm the new set is genuinely calibrated as the easier, more foundational counterpart per §2a, not harder and not just a reshuffle, this is what makes the calibration real instead of asserted.

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
- **Promoted 2026-09-24, per `generation-learnings/mcq.md`, real evidence from 5 of 5 generated files in one engagement:**
  - No em-dashes or en-dashes anywhere in a stem, option, or explanation, use commas or separate sentences instead.
  - No word or phrase from the stem echoed in exactly one option, that's a text-matching giveaway, echo it in a distractor too or drop it from the correct option.
  - No absolutist or strawman distractors ("always," "never," "completely wrong"), every wrong option needs to read as something a learner could genuinely believe from a partial understanding.

---

## 2a. Real shape per type, grounded in the actual delivered e& Orientation and Closing decks

**Corrected 2026-09-24, real finding.** The 2026-09-18 version of this section had the pre/post shape and difficulty backwards, confirmed by reading the real Orientation deck (`PowerUp | AI Builder Program (Low Code) for e& - Orientation`) and the real Closing deck (`Closing notes | e& - AI Builder Accelerator (Low Code)`) directly, both for this same engagement:

- **Pre-test** (embedded in the real Orientation deck, labeled "Pre-Program Assessment"): 10 single-correct MCQs + 2 subjective questions. **No multi-select items at all.** Genuinely the harder, more diagnostic of the two, real questions test failure-mode reasoning the learner should already have some baseline for (why fixed-character chunking degrades RAG answers, why a supervisor architecture creates a bottleneck vs. a swarm, why ReAct beats Chain-of-Thought for tool-using agents), not course content, since nothing's been taught yet, this is a baseline/diagnostic instrument, not a foundational check.
- **Post-test** (embedded in the real Closing deck, labeled "Post-Course Assessment"): 10 single-correct MCQs + 2 subjective questions. **No multi-select items at all.** Genuinely the easier, more foundational of the two, real questions check whether specific vocabulary and concepts taught over the program actually landed (what is prompt engineering, what does temperature control, what does a retriever do, what does ReAct stand for, what's the role of a Coordinator agent), recall-level, not applied/scenario-level.
- **In-session quiz**: shorter still, scoped to a single day, lighter-weight knowledge check rather than a formal assessment. Count tied to that day's real Lesson Plan structure, roughly one question per load-bearing Part/pod (the same Part-by-Part unit `slide-content-planning/SKILL.md` §3 walks), not a fixed number.

**Configurable, per `content-generation/SKILL.md` §2a (explicit human input always wins):** if a human states specific counts for a run (how many single-correct, how many multi-select, how many subjective, or a specific in-session total), that stated number is what this run builds against, full stop. Absent that, use the defaults above: **10 single-correct + 2 subjective for both pre-test and post-test, no multi-select unless explicitly asked for**, and one in-session question per load-bearing pod for that day. These are real, confirmed defaults now, not a rigid template, but they're grounded in two actual delivered instruments for this exact engagement, not a guess.

---

## 2b. After a human approves it, ask about turning it into a real form

**Added 2026-09-24, same real pattern as `onboarding-form-generation/SKILL.md` §7a, but a materially different shape, confirmed from the real live e& submission form.** The `.docx` is an authoring/review copy, not what a learner actually submits answers through.

**Once a human has approved the reviewed set, don't silently pick a format, ask.** Something like: *"This is approved, want me to turn it into the real submission form?"*

**The real form's shape is not the same as onboarding form's.** Checked directly against the actual live e& pre-test Microsoft Form: it shows a bare "Question 1," "Question 2"... label per item, with the full answer options written out underneath (so the learner can actually select one), but **never the question stem/scenario text itself**. The real question is displayed on screen and read live during the session, deliberately kept out of the form, so it can't be searched, screenshotted in advance, or answered without attending. Building the stem into the form would defeat that, don't do it even though it would technically be more complete.

- **Google Form**: buildable now, using this session's live Drive/Forms access. Build it with one item per question ("Question 1," "Question 2," ...) and the real, already-reviewed answer options under each, no stem text, matching the real live pattern exactly.
- **MS Form**: not wired up yet, no Microsoft Forms connection exists in this pipeline today. If asked for, say so plainly rather than attempting it or quietly falling back to Google.

---

## 3. Self-verify before handoff, starting with what's already strong evidence

Per `content-generation/SKILL.md` §3 and §6a, and the two candidates already seeded in `generation-learnings/mcq.md`, apply these from day one rather than waiting for three fix-loop occurrences to confirm them, the legacy evidence behind both is already stronger than the usual bar:

- **Explanation completeness.** Every question's explanation addresses why each wrong option is wrong, not only why the correct one is right. Three of four sampled legacy sets had zero explanations at all, don't repeat that gap.
- **Distractor-text independence.** No two options sharing near-identical phrasing that differs only in a trailing clause, a test-taker who spots the shared stem can eliminate one without understanding the content, a confirmed real pattern, not a hypothetical.

**Promoted 2026-09-24, set-wide checks, not just per-item:** the defects above (length giveaway, letter clustering, em-dashes, text-matching, absolutist distractors) are individually easy to miss item by item but show up clearly once you look at the whole set. Before saving, actually tally two things across the full set: which letter holds the correct answer for each item (flag it if one letter is heavily over- or under-represented), and roughly compare each correct answer's length against its own distractors (flag it if the correct answer is consistently the longest). Fix what the tally finds rather than trusting each item looked fine in isolation.

---

## 4. Where it gets saved

`Outputs/[Client]/mcq/`, matching the house format (real reference sets are `.docx`), one file per type:
- `Outputs/[Client]/mcq/pre-test.docx`
- `Outputs/[Client]/mcq/post-test.docx`
- `Outputs/[Client]/mcq/day-N-in-session.docx` (one per day, `N` = the actual day number for this engagement)

Also pushed to the `PostSalesPluginOutput` Drive folder per `content-generation/SKILL.md` §1b, same per-type filenames, mirroring this local structure. This is the working/authoring copy, not the delivered instrument, the real test learners take is a form (Orientation and Closing Ceremony hand off to that separately); this file exists so the questions get authored and reviewed properly before either deck embeds them.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:mcq-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/mcq.md` and `agent-loops` by reading it at the start of the run (skills have no `skills:` frontmatter field, only agents do). Per §6a, this is not a one-off: once a rule fails three or more times across different generated sets, per the promotion rule, it becomes a directive here, and this skill is expected to actually follow it on the next set generated, not just this one.

---

## 6. Checklist
- [ ] Type confirmed before starting (pre-test / post-test / day-N in-session), not assumed
- [ ] Both mandatory inputs loaded, or the gap handled per `content-generation/SKILL.md` §6c; pre-test read first if generating post-test
- [ ] Every item traced to a stated Learning Objective, no untaught content tested; in-session scoped to that one day only
- [ ] Single-best-answer vs multi-select format confirmed from the stem before writing distractors
- [ ] No compound claims, no implausible distractors, no cueing, no length giveaway
- [ ] Explanation completeness and distractor-text independence applied from this first run, per §3
- [ ] Answer-key format consistent (letter plus text) across every item
- [ ] Difficulty matches §2a's real, confirmed calibration: pre-test is the harder/diagnostic one, post-test is the easier/foundational one, not assumed the other way around
- [ ] Question counts match what a human explicitly stated for this run, or the real default (10 single-correct + 2 subjective, no multi-select) if nothing was stated, per §2a
- [ ] Saved to `Outputs/[Client]/mcq/<type>.docx` locally and to `PostSalesPluginOutput` per §4
- [ ] Handed to the existing `mcq-reviewer` for this specific file, no bespoke review invented
- [ ] After approval, asked whether to build the real submission form, not skipped or silently decided
- [ ] Real form, if built, has no question stem text, only the item label and real answer options, per §2b; MS Form, if requested, stated plainly as not yet available
