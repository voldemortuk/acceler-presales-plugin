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

The 33 questions, 6 sections, and MUST / SHOULD / NICE tags already exist in `skills/discovery-checklist/SKILL.md` (the pre-sales one). Score against that exact checklist, not a new or trimmed version. If a question was already answered during pre-sales (the proposal, discovery calls), pull the answer forward instead of re-asking the client. Only genuinely unanswered items need chasing at this stage.

**One adjustment for the post-sales context.** By the time this runs, the deal is signed. A few MUST items from the original checklist, budget range, who approves, when they decide, are usually already resolved by that point. Mark these as "confirmed from proposal" rather than re-scoring them as open gaps, but still record the actual figures, later stages (pricing already used them, and they're useful precedent for deep research) shouldn't have to go hunting for them again.

---

## 2. The gate, same thresholds discovery-fit-review already set

- **>=80%:** Approve, continue to Deep Research.
- **50-79%:** Comment, proceed, but every gap gets logged explicitly, not silently absorbed.
- **<50%:** Request Changes, this blocks the rest of the post-sales pipeline, same as it would in pre-sales. Don't let Deep Research or Lesson Plan start against a Thin brief.

---

## 3. The Facts Sheet, same shape discovery-fit-review already defines

Audience (who, how many, technical level), the stated 3-month success metric, tool and access constraints, timeline and format constraints, any regulated-data constraints. Extract verbatim where possible, don't paraphrase into something vaguer. The 3-month success metric is the single most load-bearing fact, same rule as the pre-sales version, flag it specifically if it's missing even when the overall score clears 80%.

---

## 4. Where it actually gets saved

`post-sales/Outputs/[Client]/discovery-facts-sheet.md`, inside the engagement's own folder under this plugin's own `Outputs/`, per `content-generation/SKILL.md` §1a, never relative to wherever the proposal or other input files happen to live. This is a new convention, starting here, every post-sales artifact for a given engagement (this sheet, the deep research doc, the lesson plan, and everything generated after it) lives under that same `post-sales/Outputs/[Client]/` folder rather than as flat files, since post-sales produces a whole family of documents per engagement, not just one.

---

## 5. Handoff

Once approved, this sheet is the required starting input for Deep Research, not optional context, per `content-generation/SKILL.md` §2. Deep Research reads it directly from `Outputs/[Client]/discovery-facts-sheet.md` rather than re-deriving audience or success metric on its own.

---

## 6. Checklist
- [ ] Scored against the existing 33-question checklist, not a new one
- [ ] Answers already known from pre-sales pulled forward, not re-asked
- [ ] Money/timeline MUST items marked "confirmed from proposal" where already resolved, with the actual figures recorded
- [ ] Facts Sheet extracted verbatim where possible, the 3-month success metric specifically checked
- [ ] Saved to `Outputs/[Client]/discovery-facts-sheet.md`
- [ ] Score <50% blocks the pipeline, doesn't just get noted and ignored
