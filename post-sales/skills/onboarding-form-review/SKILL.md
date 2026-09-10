---
name: onboarding-form-review-skills-acceler-single-artifact-panel
description: "How to review one Learner Onboarding Form for whether it's actually tied to the program's stated outcome (per the Discovery Facts Sheet), stack/environment and skill-self-assessment relevance, correct handling of the conditional Tech-Lead/team-lead section, and data proportionality — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in a real pristine reference (LVT Engineer Onboarding Form). Companion to discovery-fit-review and audience-fit-review, which consume this form's aggregate responses."
metadata:
  type: reference
---

# Acceler Onboarding Form Review — SKILL.md (v1)

Reviews **one Learner Onboarding Form**. Grounded in a real, pristine reference: LVT's "Engineer Onboarding Form" (AI-Assisted Development Program). This replaces the earlier v0 grounding (reconstructed headers from a filled e& response sheet) — that pattern is still valid corroborating evidence, but this is the actual template.

**Resolved scoping note:** earlier research concluded a "Discovery Form / Questions From Team Leads" doesn't exist anywhere in the corpus as a standalone artifact. That conclusion was correct about a *standalone* form, but incomplete: the LVT reference shows it as a real pattern **embedded as a conditional section inside the same onboarding form** ("FOR TECH LEADS · optional, one per team"), not a separate artifact. §1.4 below covers it. Don't build or expect a separate discovery-form-review skill — this is the right home for it.

---

## 1. Rules

### 1.1 Outcome-Tie (the load-bearing rule)
*Confirmed pattern, now from a pristine reference, not just reconstructed headers: "What is your primary goal for this program?" with a worked example prompt ("Use Claude Code confidently on our real codebase — plan-mode, review, and safe refactors without slowing my team down"). Also: "Biggest concern about using AI agents on your real codebase?" — a second, distinct outcome-adjacent question worth checking for.*
- The form must include at least one open-ended question tied to the program's stated success metric/outcome, sourced from `discovery-fit-review`'s Discovery Facts Sheet. A form that's only identity fields + generic skill ratings, with no outcome-tied open question, is a FAIL.
- A strong form (per the LVT reference) also asks what the learner is most **concerned about** — this surfaces resistance/risk the program should address, not just aspiration. Its absence isn't automatically a FAIL, but flag it as thinner than the house standard.
- If the Discovery Facts Sheet isn't available, say so plainly and flag that this check can't run with confidence — never invent a plausible-sounding outcome to check the form against.

### 1.2 Stack & Environment Coverage *(for technical/engineering cohorts)*
*New section grounded in the LVT reference's "Your Stack & Environment" block — languages, primary IDE, OS, and dev-environment shape (local/devcontainer/cloud). This didn't exist in the earlier e&-only grounding; e&'s non-technical/business cohorts don't need it, LVT's engineer cohort does.*
- For a technical/engineering audience, the form should capture the actual languages/stack the cohort works in day-to-day, plus environment shape — this is what lets a bootcamp repo and labs be built to match reality instead of a generic stack. Its absence for an engineering cohort is a FAIL; its absence for a non-technical audience is not applicable.

### 1.3 Skill Self-Assessment Relevance
*Grounded in the LVT reference's per-tool familiarity matrix (rows: AI chat, AI code editors, Claude Code/CLI agents, Git CLI, CI/CD, MCP servers — each rated Never/Beginner/Intermediate/Advanced) alongside self-rated language proficiency and production-ownership experience.*
- Skill/tool familiarity questions should cover the tools the Discovery Facts Sheet says this specific client's cohort will actually use. A form padded with irrelevant tool questions, or missing the tools that matter for this engagement, is a FAIL either direction.
- Prefer a rated matrix (Never/Beginner/Intermediate/Advanced per tool) over a single blanket familiarity question where multiple tools are in scope — it's what lets pacing be calibrated per-tool rather than assuming uniform familiarity.

### 1.4 Conditional Tech-Lead/Team-Lead Section *(when the engagement has a lead/manager layer)*
*Grounded in the LVT reference's "FOR TECH LEADS · optional, one per team" section: monorepo/polyrepo and repo size/age, build & test tooling and suite runtime, CI system and whether a hook/check can be wired into it during a lab, current PR workflow (review gates, required checks, branch protection).*
- Where the engagement has a distinguishable lead/manager audience layer, the form should include a clearly-marked optional section for them, collecting the architecture/process detail an individual contributor wouldn't know or wouldn't be asked to answer — this is what lets hands-on labs mirror the client's *real* environment (their actual CI, their actual PR gates) instead of a generic one.
- This section must be explicitly optional and clearly scoped ("skip this if you are not a X"), not silently mixed into the main flow where a non-lead might feel obligated to answer it.

### 1.5 Data Proportionality
- Only collect personal data actually used downstream — identity fields (name, email, role, team/function) are fine; anything more invasive needs a stated reason tied to program design. The LVT reference explicitly states "no production code or secrets are ever requested" — a form for a technical/code-adjacent audience should carry an equivalent explicit reassurance, not leave it implied.

---

## 2. How this runs

- Launched as the `acceler-post-sales:onboarding-form-reviewer` agent — read-only by tool restriction.
- Given: the onboarding form, the Discovery Facts Sheet from `discovery-fit-review`, and whether this engagement has a lead/manager audience layer (for §1.4). If the Facts Sheet is missing, run §1.2-1.5 only and say plainly that §1.1 couldn't run with confidence.
- Output: `{question/section, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Onboarding-form-specific only:

- "The artifact" = the form itself. The fixer may touch only the form — never the Discovery Facts Sheet or this skill's §1 rubric.
- Dismissed findings logged to §4 Memories below, not re-surfaced.

---

## 4. Memories

```
- [date] Dismissed: <finding> — <human's stated reason>
```

(empty until first use)

---

## 5. Verdict

| Verdict | Condition |
|---|---|
| ✅ Approve | All applicable §1 rules PASS, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain, or §1.1 couldn't run for lack of a Facts Sheet |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Discovery Facts Sheet checked for and used if available; its absence explicitly flagged if not
- [ ] Outcome-tie question checked against the *actual* stated success metric, not a generic assumption
- [ ] §1.2/§1.4 applicability correctly determined by audience type (technical cohort / has a lead layer), not applied blanket
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
