---
name: onboarding-form-generation-skills-acceler-learner-intake
description: "Generates one Learner Onboarding Form per engagement, one common baseline tweaked per client, not rebuilt from scratch. Includes the conditional lead-only section inline when the engagement has a distinguishable lead/manager audience, per the resolved finding that this is not a separate artifact. Grounded in two real references, LVT (engineering-heavy) and e& (mixed/business), showing how the same skeleton flexes by audience. Routes to the existing onboarding-form-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Onboarding Form Generation · Post-Sales · SKILL.md (v1)

**What this produces.** One Learner Onboarding Form, the same artifact `onboarding-form-review/SKILL.md` already reviews. One common baseline, tweaked per client, not designed from scratch each time. **Not a separate "Discovery Form for Team Leads"**, per that reviewer's own resolved note: where the engagement has a lead/manager layer, that's a clearly-marked optional section inside this same form, not a standalone artifact.

**Grounded in two real references, not one, deliberately.** `onboarding-form-review` documents LVT's Engineer Onboarding Form, a purely engineering cohort (stack, IDE, Git, CI, PR workflow). A second real reference, e&'s AI Builder Accelerator form, shows the same skeleton used for a mixed, often non-technical cohort (directors and managers with no coding background answering the same form as engineers). Comparing them is what shows which parts of the baseline are universal and which flex by audience.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/discovery-facts-sheet.md`, approved. The outcome-tie question, the single most load-bearing question on the whole form, is sourced from this, never invented. **Strengthened 2026-09-18, per `content-generation/SKILL.md` §2:** if the Facts Sheet's stated success metric or goal is itself thin or generic, that's a granular gap within an otherwise-present stage, not a reason to stop. Flag it plainly in the output rather than quietly writing a vague outcome-tie question that reads as if it came from something specific.

**Best-effort:**
- Whether this engagement has a distinguishable lead/manager audience layer, from deep research or the proposal, decides whether the conditional section (§4) gets included.
- A similar past onboarding form as a reference, where one exists for a similar client or program type.

---

## 2. The baseline, universal across both real references

- **Identity**: name, company email, designation/job title, years of professional experience.
- **Outcome-tie question**, sourced from the Facts Sheet, "what is your primary goal for this program," with a worked example answer to anchor response quality, not a vague open field. This is the one question a form cannot ship without.
- **Concern question**, "what concerns do you have," surfaces resistance and risk, not just aspiration.
- **Skill self-assessment**, a rated matrix (e.g. Never / Basic / Intermediate / Advanced), scoped to the actual tools this client's cohort will use, per the Facts Sheet, never a generic tool list copied wholesale from another engagement.
- **Topics of interest**, multi-select, what they're most excited to learn.
- **Data proportionality reassurance**, an explicit line stating no production code or secrets are ever requested, especially for a technical or code-adjacent audience.

---

## 3. Real workflow context, confirmed valuable, not in the reviewer's rubric yet

*New observation from the e& reference, worth building in even though `onboarding-form-review` doesn't check for it explicitly yet: two questions asking what the learner actually does day to day, and one repetitive task they wish were automated.* This is where real, client-specific use cases actually enter the system, deep research and later the demo/assignment generators lean on exactly this kind of detail to avoid generic examples. Include it in the baseline even though it's not yet a reviewer-enforced rule.

---

## 4. The conditional lead-only section, inline, not a separate form

Include only when this engagement has a distinguishable lead/manager audience layer. Clearly marked optional, explicitly scoped ("skip this if you are not a team lead or manager"), never silently mixed into the main flow. Collects what an individual contributor wouldn't know: monorepo vs. polyrepo and repo size/age, build and test tooling and suite runtime, CI system and whether a check can be wired in during a lab, current PR review gates and branch protection. This is what lets hands-on labs mirror the client's real environment instead of a generic one.

**Audience calibration, confirmed by comparing both references.** LVT's cohort (pure engineers) gets a full stack/environment section as part of the main flow, not just the lead section. e&'s cohort (mixed, several with no coding background) skips deep IDE/Git detail and keeps the main flow lighter, technical depth lives only in the skill-matrix ratings, not in an assumed-technical main flow. Calibrate which depth applies from the Facts Sheet's stated audience, don't default to either extreme.

---

## 5. Self-verify before handoff

Per `content-generation/SKILL.md` §3: confirm the outcome-tie question is actually built from this engagement's real Facts Sheet content, not a template placeholder, and confirm the skill-matrix tool list matches what the Facts Sheet says this cohort will actually use, not a copied list from a prior engagement.

---

## 6. Where it gets saved

`Outputs/[Client]/onboarding-form.docx`, same per-engagement folder as everything else.

---

## 7. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:onboarding-form-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `agent-loops` via the `skills:` frontmatter field. No `generation-learnings/onboarding-form.md` file exists yet, and per §6a, this skill's own findings, once real fix-loop history accumulates, are expected to change how the next form gets written, not just fix the one in front of you.

---

## 7a. After a human approves it, ask about turning it into a real form

**Added 2026-09-23.** The `.docx` is an authoring/review copy, not what a learner actually fills out, same real pattern already true for MCQ (the doc gets authored and reviewed, but the real thing learners interact with is an actual form, confirmed from real Orientation/Closing Ceremony decks pointing at a Microsoft Form link). This form is no different.

**Once a human has approved the reviewed content, don't silently pick a format, ask.** Something like: *"This is approved, want me to turn it into a real Google Form or an MS Form so it's ready to send out?"* This is a genuine choice, not a default, and asking it also functions as a reminder to the human that this step still needs doing, don't let it get forgotten.

- **Google Form**: buildable now, using the same live Drive/Forms access this session already has. Build it with matching sections and question types (short answer, rating matrix, multi-select) from the approved doc content, then share the real, live submission link back.
- **MS Form**: not wired up yet, no Microsoft Forms connection exists in this pipeline today. If asked for, say so plainly rather than attempting it or quietly falling back to Google, this is real, parked follow-up work, not something to fake.

---

## 8. Checklist
- [ ] Mandatory input (approved Facts Sheet) loaded, or the run stopped and asked
- [ ] Outcome-tie question built from this engagement's real Facts Sheet content, not a placeholder
- [ ] Concern question present, not just aspiration-only questions
- [ ] Skill-matrix tool list matches what this cohort will actually use, not copied wholesale
- [ ] Real workflow/repetitive-task questions included per §3
- [ ] Lead-only section included and clearly marked optional only when this engagement actually has that audience layer
- [ ] Depth calibrated to audience (technical vs. mixed), not defaulted to either extreme
- [ ] Data proportionality reassurance present
- [ ] Saved to `Outputs/[Client]/onboarding-form.docx`
- [ ] Handed to the existing `onboarding-form-reviewer`, no bespoke review invented
- [ ] After approval, asked whether to build a real Google Form or MS Form, not skipped or silently decided
- [ ] MS Form, if requested, stated plainly as not yet available, not faked or silently swapped to Google
