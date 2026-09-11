---
name: project-generation-skills-acceler-capstone-multi-milestone
description: "Generates one capstone/multi-milestone project, either the milestone/trap-graded shape or the documentation-brief shape, whichever the engagement calls for. Builds in the compliance-vs-verified-enforcement distinction from day one rather than waiting for review to catch a structural gap, per the already-seeded generation-learnings candidate. Routes to the existing project-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Project Generation · Post-Sales · SKILL.md (v1)

**What this produces, in one of two confirmed real shapes.** The same artifact `project-review/SKILL.md` already reviews, a capstone or multi-milestone project, distinct from a single assignment: multi-stage or otherwise substantial, and graded through planted traps or a structured brief rather than a straightforward rubric. Don't force one shape's structure onto the other:

1. **Milestone/trap-graded**, staged (M1 through M4 or similar), graded via deliberately planted traps, backed by a verified reference-solution repo as the actual answer key.
2. **Documentation-brief**, a single cohesive brief: Motivation, Objectives, a role-relevance mapping (how this project matters to each function in the cohort, PM/TPM/SDE/Engineering Manager/DevOps each get their own sentence), Dataset with a source link, Prerequisites with doc links, copy-pasteable setup steps, Milestones as work areas rather than graded checkpoints, tooling used, Future Directions, and a Common FAQs section.

Which shape depends on the engagement, deep research's precedent notes and the lesson plan's stated scale/format are the signal, not a default.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved, for the capstone's stated objective and Bloom's level (Apply/Analyze/Create/Evaluate, a program's highest cognitive tier).
- `Outputs/[Client]/deep-research.md`, for the client's use cases to ground the project in, and the audience's functional mix, which decides whether a role-relevance mapping is needed.

**Best-effort:**
- An existing project of the same shape as a structural reference.

---

## 2. Rules, reused from project-review, not reinvented

**If milestone/trap-graded:**
- Explicit milestones with stated deliverables per stage, a multi-stage project with no milestone breakdown is not acceptable, though a genuinely single-session project may skip this.
- A complete, passing reference solution must actually exist, not just be described, it's the proof the project as specified is buildable at all.
- Any starter/checkpoint scaffold is explicitly labeled with which milestone it represents.

**If documentation-brief:**
- Motivation and Objectives specific to this project, not boilerplate reused from another one.
- Role-relevance mapping included whenever the cohort spans multiple functions, its absence for a mixed-audience project is a real gap, not a stylistic choice.
- Dataset section links the actual source and states whether an alternative open dataset may be substituted.
- Setup steps are concrete and copy-pasteable, exact commands, not paraphrased instructions.
- A Common FAQs section anticipates the real questions a learner would ask before starting, scope, input/output shape, audience.

**Applies to both shapes:**
- No real API keys, credentials, or secret-shaped strings. No real customer or PII data. No license-incompatible copied code.

---

## 3. The single most important rule, built in from day one

*Per the seeded candidate in `generation-learnings/project.md`, applied here without waiting for three fix-loop occurrences, the gap is structural, not a minor omission.*

**"It complied" is not the same as "I saw it get blocked."** Every planted trap or constraint in the scenario needs an explicit, checkable pass/fail criterion that forces and verifies an actual enforcement event, not just checks whether the final output happened to comply. A trap embedded in the scenario with no corresponding grading criterion that actually witnesses the block is dead weight, build the enforcement check in at the same time you plant the trap, don't plant traps first and add grading later.

---

## 4. Self-verify before handoff

Per `content-generation/SKILL.md` §3: for the milestone/trap shape, actually build and run the reference solution's test suite before saving, confirm it passes, don't hand off a guide whose own answer key was never executed. For the documentation-brief shape, confirm the role-relevance mapping is present whenever deep research's audience signal shows more than one function in the cohort.

---

## 5. Where it gets saved

`Outputs/[Client]/project/`, holding the guide, the reference solution (milestone/trap shape), and any starter scaffold, same per-engagement parent folder as everything else.

---

## 6. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:project-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/project.md` and `agent-loops` via the `skills:` frontmatter field. Per §6a, once real fix-loop history accumulates beyond the one seeded candidate above and the promotion rule is met, this skill is expected to actually change how it builds the next project, not just fix the one in front of you.

---

## 7. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Correct shape chosen (milestone/trap vs. documentation-brief) from the engagement's signal, not defaulted
- [ ] Every planted trap has an explicit, forced-and-observed enforcement criterion, not a compliance-only check
- [ ] Reference solution actually built and its test suite run, for the milestone/trap shape
- [ ] Role-relevance mapping present whenever the cohort spans multiple functions
- [ ] Bloom's level is Apply/Analyze/Create/Evaluate, matching capstone-tier work
- [ ] No real credentials, PII, or license-incompatible code
- [ ] Saved to `Outputs/[Client]/project/`
- [ ] Handed to the existing `project-reviewer`, no bespoke review invented
