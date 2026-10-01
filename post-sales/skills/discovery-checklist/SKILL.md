---
name: discovery-checklist-skills-acceler-post-sales-facts-sheet
description: "Runs the existing 33-question discovery checklist fresh at the start of post-sales, and actually saves its output this time. Produces and persists the Discovery Facts Sheet that deep research, and every downstream reviewer via agent-loops §2b, checks against. First stage in the per-engagement post-sales pipeline, right after a deal closes."
metadata:
  type: reference
---

# Acceler Discovery Checklist · Post-Sales · SKILL.md (v1)

**Why this exists.** `discovery-fit-review` already defines what the Discovery Facts Sheet contains and that downstream reviewers need it. What it doesn't solve is that the sheet isn't actually being saved anywhere on the pre-sales side, the checklist doesn't get used consistently in the first several client calls. Rather than chase something that doesn't reliably exist, this skill runs the checklist fresh, right at the start of post-sales, once a deal has closed, and persists the result somewhere real.

---

## 1. Reuse the existing checklist, don't reimplement it

The 33 questions, 6 sections, and MUST / SHOULD / NICE tags already exist in the pre-sales plugin's own checklist skill, the repo-root `skills/discovery-checklist/SKILL.md` (not this file, which sits at `post-sales/skills/discovery-checklist/SKILL.md`). Score against that exact checklist, not a new or trimmed version. If a question was already answered during pre-sales (the proposal, discovery calls), pull the answer forward instead of re-asking the client. Only genuinely unanswered items need chasing at this stage.

**One adjustment for the post-sales context.** By the time this runs, the deal is signed. A few MUST items from the original checklist, budget range, who approves, when they decide, are usually already resolved by that point. Mark these as "confirmed from proposal" rather than re-scoring them as open gaps, but still record the actual figures, later stages (pricing already used them, and they're useful precedent for deep research) shouldn't have to go hunting for them again.

**Self-check pricing figures before presenting them.** Found on the same real test run: a per-person price and a cohort total were pulled from different columns of the same pricing sheet (one list price, one discounted) and presented together as if they were paired, per-person × headcount didn't actually equal the stated cohort total. Before stating a per-person price alongside a cohort or program total, multiply them out and confirm they agree, same self-verification discipline `content-generation/SKILL.md` §3 already requires for mechanically checkable facts like duration math.

**Reading a real roster, watch for section headers that mean "not attending."** Found on the same real test run: a participant tracker had three sections, a selected list, a "Not Selected" list, and an "Opted Out" list, and a headcount was reported by summing every name in the sheet, including the rejected and opted-out ones, producing a number nearly double the real confirmed headcount and wrongly suggesting a pricing mismatch. When reading a real roster or tracker for headcount, only count names under a section that's actually confirmed/selected/attending, and check for headers like "Not Selected," "Opted Out," "Waitlist," or "Declined" before totaling anything.

---

## 2. The gate, same thresholds discovery-fit-review already set

- **>=80%:** Approve, continue to Deep Research, **unless the 3-month success MUST item is unanswered, which is Request Changes regardless of the overall score** (the same override `discovery-fit-review/SKILL.md` §5 applies, stated here too as of 2026-10-01 so the two agree).
- **50-79%:** Comment, proceed, but every gap gets logged explicitly, not silently absorbed.
- **<50%:** Request Changes, this blocks the rest of the post-sales pipeline, same as it would in pre-sales. Don't let Deep Research or Lesson Plan start against a Thin brief. This block applies to the full pipeline: a human who only wants one artifact adapted or built standalone can still get it through `content-generation/SKILL.md` §6c's light brief, with the thin discovery stated plainly in that brief.

---

## 3. The Facts Sheet, same shape discovery-fit-review already defines

Audience (who, how many, technical level), the stated 3-month success metric, tool and access constraints, timeline and format constraints, any regulated-data constraints, and **Branding** (Acceler by default, PowerUp only if a human asks, recorded here once so no later stage has to ask again, per `content-generation/SKILL.md` §1d and `discovery-fit-review/SKILL.md` §1.2). Extract verbatim where possible, don't paraphrase into something vaguer. The 3-month success metric is the single most load-bearing fact, same rule as the pre-sales version, flag it specifically if it's missing even when the overall score clears 80%.

**Don't confuse this with an end-of-program learning metric.** Confirmed happening on a real test run (e& AI Builder Low Code, 2026-09-13): a proposal's own "Success Metrics" section (pre/post assessment improvement, capstone completion rate, session feedback scores) measures whether the training itself worked, not what changed in the client's business afterward. A true 3-month success metric is a stated business outcome tracked after the program ends, something like "reduce ticket resolution time by X%" or "Y% of participants ship an agent to production." If the source material only has end-of-program training metrics, the 3-month metric is genuinely missing, log it as a gap, don't mark it present just because some kind of success metric exists.

---

## 4. Where it actually gets saved

`post-sales/Outputs/[Client]/discovery-facts-sheet.md`, inside the engagement's own folder under this plugin's own `Outputs/`, per `content-generation/SKILL.md` §1a, never relative to wherever the proposal or other input files happen to live. This is a new convention, starting here, every post-sales artifact for a given engagement (this sheet, the deep research doc, the lesson plan, and everything generated after it) lives under that same `post-sales/Outputs/[Client]/` folder rather than as flat files, since post-sales produces a whole family of documents per engagement, not just one.

**If a Facts Sheet already exists somewhere else, don't edit it in place.** Confirmed happening on a real test run: a prior pass had saved a sheet outside the plugin (next to the source files, following the old unstated convention), and a later run found it and kept updating that same file rather than moving to the correct location. The correct location is always `post-sales/Outputs/[Client]/discovery-facts-sheet.md`, full stop. If an older sheet is found anywhere else, treat it as reference material for what's already been figured out, but save the current run's output to the correct location, don't perpetuate the wrong one.

---

## 5. Handoff

Once approved, this sheet is the required starting input for Deep Research, not optional context, per `content-generation/SKILL.md` §2. Deep Research reads it directly from `post-sales/Outputs/[Client]/discovery-facts-sheet.md` rather than re-deriving audience or success metric on its own.

---

## 6. Checklist
- [ ] Scored against the existing 33-question checklist, not a new one
- [ ] Answers already known from pre-sales pulled forward, not re-asked
- [ ] Money/timeline MUST items marked "confirmed from proposal" where already resolved, with the actual figures recorded
- [ ] Facts Sheet extracted verbatim where possible, the 3-month success metric specifically checked
- [ ] Saved to `post-sales/Outputs/[Client]/discovery-facts-sheet.md`, not left at or copied from an old wrong-location file
- [ ] Any roster/headcount figure checked for "Not Selected"/"Opted Out"/similar non-attending sections before totaling
- [ ] Score <50% blocks the pipeline, doesn't just get noted and ignored
