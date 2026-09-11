---
name: assignment-generation-skills-acceler-rubric-graded-task
description: "Generates one open-ended, rubric-graded assignment per session, with a weighted rubric, an explicit starter-kit split where one is provided, and setup/security checks handled at creation time for code-based assignments. Routes to the existing assignment-reviewer for the actual review pass. Loops with it over time: promoted generation-learnings directives are expected to change how the next assignment gets written, not just fix the one in front of you."
metadata:
  type: reference
---

# Acceler Assignment Generation · Post-Sales · SKILL.md (v1)

**What this produces.** One open-ended, rubric-graded assignment, the same artifact `assignment-review/SKILL.md` already reviews. Kept separate from MCQ generation, rubric and answer-key defects are a different failure mode from item-writing defects, and separate from project generation, which covers multi-milestone capstones rather than a single task.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved. The assignment's objective and Bloom's level come from the relevant day's row, assignments sit at Apply/Analyze/Create, not Remember/Understand.
- `Outputs/[Client]/deep-research.md`, for audience calibration and, where relevant, the client's own use cases to ground the task in.

**Best-effort:**
- That day's generated code-demo, if one already exists, the assignment should build on the same tool or technique, not a disconnected task.
- An existing assignment template or gradesheet as a formatting reference.

---

## 2. Rules, reused from assignment-review, not reinvented

Generate against what `assignment-review/SKILL.md` §1 checks for:

- **Weighted rubric, not a bare description.** The house shape is a weighted table, criteria plus percentages (the confirmed real pattern: Functionality 30%, Technical Implementation 25%, UX 20%, Setup & Reproducibility 10%, Documentation 10%, adjust weights to the actual task, but keep the shape). A rubric with no weights or no stated criteria is not acceptable.
- **If a starter kit is provided, state the split explicitly.** How much is pre-built versus what's actually graded, e.g. "this gives you roughly 40% of the work." Never hand over a starter kit silently.
- **Subjective/open-ended items still need scoring criteria.** Either a full model answer, or tips plus an explicit marks breakdown. Tips alone with nothing to score against is not acceptable.
- **Bloom's level.** Apply, Analyze, or Create, matching or exceeding the mapped objective, not a Remember/Understand-level task dressed up as an assignment.

**Conditional, only when the assignment is code-based:**
- No real API keys, credentials, or secret-shaped strings, ever, even as placeholders that look real.
- No real customer or PII data, sample data must read as obviously synthetic.
- No license-incompatible copied code.
- Every provided data file described (format, and whether it holds text, tables, or images).
- Required languages, libraries, and prerequisites stated up front, not left for the learner to discover.
- Execution environment specified, local versus hosted notebook, and any hardware expectations, wherever the work has real compute needs.
- Even for open-ended, research-style tasks, provide a few starting reference links, full silence on where to start is not acceptable.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3: before saving, confirm the rubric's weights actually sum to 100%, and that the conditional code-based checks in §2 were run if, and only if, this assignment is code-based, don't skip them for a code assignment or apply them needlessly to a written-answer one.

---

## 4. Where it gets saved

`Outputs/[Client]/assignment.docx`, plus a `starter-kit/` subfolder alongside it where a code-based assignment provides one, same per-engagement folder as everything else.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:assignment-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/assignment.md` and `agent-loops` via the `skills:` frontmatter field. No candidates are seeded there yet, unlike MCQ and Lesson Plan, but per §6a, once real fix-loop history accumulates and the promotion rule is met, this skill is expected to actually change how it writes the next assignment, not just fix the one in front of you.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Rubric is weighted, criteria stated, weights sum to 100%
- [ ] Starter-kit split stated explicitly where a starter kit is provided
- [ ] Subjective items have real scoring criteria, not tips alone
- [ ] Bloom's level is Apply/Analyze/Create, matching or exceeding the objective
- [ ] Code-based conditional checks (§2) run only when the assignment is actually code-based
- [ ] Saved to `Outputs/[Client]/assignment.docx`, starter kit alongside it if one exists
- [ ] Handed to the existing `assignment-reviewer`, no bespoke review invented
