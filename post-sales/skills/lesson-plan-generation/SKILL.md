---
name: lesson-plan-generation-skills-acceler-facilitator-table
description: "Generates the internal, minute-level facilitator planning table (Topic/Objective/Subtopic/Time/Flow/Demo/Tools per row, one tab per day) that precedes content creation. Builds from deep research and the pre-sales proposal, self-checks its own timing math before handoff, and routes to the existing lesson-plan-reviewer for the actual review pass. First content-generation artifact in the pipeline, everything after it (slides, demo, MCQ, assignment, project, and instructor finalization) reads its day-by-day structure. This loops with lesson-plan-reviewer over time, not just once: its findings feed generation-learnings/lesson-plan.md once the promotion rule is met, and are expected to make the next generated Lesson Plan better, not just fix the one in front of you."
metadata:
  type: reference
---

# Acceler Lesson Plan Generation · Post-Sales · SKILL.md (v1)

**What this produces.** The same document `lesson-plan-review/SKILL.md` already reviews, the internal facilitator planning table, not the client-facing Day-by-Day proposal section and not the session deck. Confirmed real structure from sampled files: an Excel file (xlsx), one tab per day for multi-day programs, one row per curriculum segment, columns `Topic | Learning Objective | Subtopic | Estimated Time | Flow of Examples & Topics | Live Demo/Coding Demo | Libraries/Tools`.

---

## 1. Inputs

Per `content-generation/SKILL.md` §2's tiering.

**Mandatory:**
- `Outputs/[Client]/deep-research.md`, approved.
- The pre-sales proposal's day-by-day section, the starting skeleton of themes per day. **Added 2026-09-18, per `content-generation/SKILL.md` §2: the single most load-bearing granular fact here is the actual day count and per-day duration this proposal states.** Not always 4 days, real engagements run 3, 4, 5, or other lengths, and §3's real duration-mismatch incident shows what happens when this figure isn't pinned to the source. If the proposal states it ambiguously (a range, or duration implied rather than stated), don't stop, but name the ambiguity plainly and state which reading this run used.

**Best-effort:**
- Acceler's existing curriculum/topic library, whatever module content already exists to distill into rows, use it where available, flag plainly where a day's content has to be drafted without a matching existing module.
- A similar past Lesson Plan as a formatting and depth reference, where deep research's precedent notes point to one.

---

## 2. Structure rules, reused from lesson-plan-review, not reinvented

Since generation and review need to agree on what "correct" means, these come straight from `lesson-plan-review/SKILL.md` §1, stated here only as generation-time directives:

- Seven columns, populated per row. Break/lunch rows (Topic = a time range, rest blank) and AMA rows (Subtopic = `-`, Flow blank) are legitimate, not gaps to fill in. **Break and lunch time is included within the day's stated total, never added on top of it.** Confirmed against the real Tech Teams Lesson Plan (`Lesson Plans for Tech __ e&.xlsx`), every break/lunch row there sits inside one continuous day window, start to finish, nothing gets tacked on past the proposal's stated hours. A "6 hour day" means 6 hours total including breaks, not 6 hours of content plus extra break time.
- One timing format per document, clock-ranges or raw durations, never mixed across days.
- Every module covers pre-class, live-class, and post-class as distinct, identifiable rows, not just the live-class content.
- Every Learning Objective's Bloom's verb is matched or exceeded by that row's Demo content, and every objective is reflected somewhere in Flow or Demo, no orphaned objectives.
- Demo weight stays light on introductory days where a later day is explicitly dedicated to hands-on building, don't front-load the heavy build before its dedicated slot.
- Libraries/Tools per row matches what that day's actual code-demo or hands-on-guide artifact uses, where those already exist.

**Depth calibration, not in the reviewer's scope but real per Utkarsh's own note.** A dense, hands-on-heavy template applied to a leadership or non-technical audience reads as verbose. Use deep research's audience signal (from the Facts Sheet, carried through) to decide row density and demo depth, don't apply one template regardless of audience.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3, and directly from the seeded candidate in `generation-learnings/lesson-plan.md`: compute each day's row-duration sum and check it against that day's stated session length, before presenting or saving. This exact gap was found in every previously sampled Lesson Plan, none of them self-checked it. Don't ship a plan with unverified timing math and let review catch it, catch it here.

**The target for that check is the proposal's own literal stated duration, never a number this run assumes for itself.** Confirmed happening on a real test run (e& AI Builder Low Code, 2026-09-13): the proposal stated "6 Hours Each," the generated plan added a 45-minute break on top of that and self-verified against its own 6h45m total instead, both the generation self-check and the independent reviewer confirmed the rows summed correctly, neither caught that the total itself didn't match the real stated duration. Rows summing to themselves consistently is not the same as summing to the real number, check against the actual source figure (the proposal's stated hours, or real confirmed delivery clock times where they exist), not a total the run invented and then verified against itself.

---

## 4. Where it gets saved

`Outputs/[Client]/lesson-plan.xlsx`, same per-engagement folder as the Facts Sheet and deep research doc.

---

## 5. Handoff, and this is a loop, not a one-time pass

Once the self-check in §3 passes, hand off to the existing `acceler-post-sales:lesson-plan-review` for the actual review pass, per `content-generation/SKILL.md` §6. This skill does not define its own review process. Reference `generation-learnings/lesson-plan.md` and `agent-loops` (for §2a baseline quality bar and §2a-1 prose quality, this is a facilitator-facing document, the same writing-quality bar still applies) via the `skills:` frontmatter field.

This connection is not a one-off handoff. Per `content-generation/SKILL.md` §6a, when the same rule fails three or more times across different generated Lesson Plans, per `generation-learnings/README.md`'s promotion rule, that becomes a directive in `generation-learnings/lesson-plan.md`, and this skill is expected to actually follow it on every future run, not just this one. Fixing the plan in front of you and stopping there is not the goal, the next Lesson Plan generated should already avoid whatever pattern kept failing.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked rather than guessing
- [ ] Seven-column structure per row, break/lunch/AMA rows correctly left as exceptions
- [ ] Pre-class, live-class, post-class present as distinct segments per module
- [ ] Every objective has matching or exceeding Demo content, nothing orphaned
- [ ] Row-duration sums checked against the proposal's actual stated day length, not a total this run assumed and then verified against itself
- [ ] Break/lunch time confirmed included within that stated total, not added on top of it
- [ ] Depth calibrated to audience from deep research's Facts Sheet signal, not one template applied regardless
- [ ] Saved to `Outputs/[Client]/lesson-plan.xlsx`
- [ ] Handed to the existing `lesson-plan-review`, no bespoke review process invented
