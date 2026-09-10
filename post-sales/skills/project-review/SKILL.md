---
name: project-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler capstone/multi-milestone project (facilitator + learner guide, planted-bug grading signals, reference-solution answer key) — distinct from a simple assignment: multi-stage, trap-based grading, and a critical rubric rule distinguishing compliance from verified enforcement. Human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Companion to assignment-review, code-demo-review, and the content-review bundle."
metadata:
  type: reference
---

# Acceler Project Review — SKILL.md (v1)

Reviews **one capstone/multi-milestone project** in isolation. Kept separate from `assignment-review` — confirmed via a real reference (`B2B-LVT Pro Code Program - Claude/.../Project Operation Stale Orders - Guide.docx`) that a Project is structurally a different artifact: milestone-staged (not single-deliverable), graded via deliberately planted traps rather than a straightforward rubric, and backed by a verified reference-solution repo as the actual answer key. Conflating this with `assignment-review` would dilute both rubrics.

---

## 1. Rules

**Two confirmed, equally legitimate house shapes for a project.** Don't force one shape's checklist onto the other:
1. **Milestone/trap-graded** (LVT's Stale Order Alerts project) — staged (M1-M4), graded via deliberately planted traps, backed by a verified reference-solution repo. §1.1-1.2 below apply.
2. **Documentation-brief shape** (confirmed real, e.g. an "Automated Insights Generation" project doc) — a single cohesive brief: Motivation → Objectives → **role-relevance mapping** (a real, distinctive section: "How is Today's Content Relevant to my Role" — a per-audience-function breakdown, e.g. PMs/TPMs/SDEs/Engineering Managers/DevOps each get one sentence on why this project matters to their role) → Dataset (with source link) → Prerequisites (languages/frameworks, each with a doc link) → "Get Ready for the Session" setup steps (numbered, copy-pasteable commands) → Milestones (as work areas, not graded checkpoints) → Model/tooling used → Future Directions → Common FAQs. Use §1.1a below for this shape instead of §1.1.

### 1.1 Milestone Structure & Completeness *(milestone/trap shape)*
- Multi-stage projects must have explicit milestones with stated deliverables per stage (reference shape: M1 discovery/contract interview → M2 plan-mode design decisions → M3 build → M4 verify). A multi-day/multi-stage project with no milestone breakdown is a FAIL. A genuinely single-session project may legitimately skip this — don't force-fit milestones onto something that doesn't need them.
- A **verified reference solution** must exist — a complete, passing implementation (the reference set's `final-complete-project/` passes 41 tests) that proves the project as specified is actually achievable. A project guide with no reference solution is a FAIL — there's no way to confirm the brief itself is buildable, let alone gradeable.
- If a starter/checkpoint scaffold is provided for a time-boxed live format (the reference set's `session-starter/` = an M1-3-done checkpoint for a 2-hour session), it must be explicitly labeled with which milestone it represents — an unlabeled partial scaffold is a FAIL, learners and facilitators can't tell what's already done.

### 1.1a Documentation-Brief Completeness *(documentation-brief shape)*
- Motivation and Objectives sections present and specific to this project, not generic boilerplate reused across projects.
- **Role-relevance mapping present** if the cohort spans multiple functions — a real, distinctive house pattern; its absence for a mixed-audience project is a FAIL, since it's how a learner outside the "obvious" target role understands why this project matters to them.
- Dataset section links to the actual source, and states whether learners may substitute an alternative openly-available dataset.
- Setup steps ("Get Ready for the Session") are concrete and copy-pasteable (exact commands, not paraphrased instructions) — same standard as `code-demo-review` §1.1's no-code-guide check.
- A Common FAQs section addresses the questions a learner would actually ask before starting (scope, input/output shape, audience) — a project with zero anticipated-question coverage is thinner than the house standard.

### 1.2 Grading-Signal Validity (planted-bug/trap system)
*Grounded in the reference project's own core lesson, quoted directly because it's the load-bearing rule in this skill: **"it complied" ≠ "I saw it get blocked."***
- **The rubric must distinguish an agent voluntarily avoiding a constraint violation from a guardrail actually being triggered and observed blocking one.** A rubric that only checks final-output compliance, without ever forcing and verifying an actual enforcement event, is a FAIL — this is the single most important check in this skill. Compliance without a witnessed block proves nothing about whether the guardrail works.
- Every planted trap/constraint in the project scenario (e.g. a PII-policy violation, a schema-mismatch reference table, a dedupe-namespace collision — real examples from the reference project) must have an explicit, checkable pass/fail criterion in the rubric. A trap embedded in the scenario with no corresponding grading criterion is dead weight — flag it.

### 1.3 Curriculum & Objective Alignment
- Same Bloom's-verb-match rule as `deck-review` §1.2, but scoped to capstone level: projects are typically a program's highest cognitive tier (Apply/Analyze/Create/Evaluate). Flag a project that only demands lower-order work — that's a project in name only.

### 1.4 Security & Data-Boundary
- Same as `code-demo-review` §1.4 — projects are typically code-based. No real API keys/credentials, no real customer/PII data, no license-incompatible copied blocks.

---

## 2. How this runs

- Launched as the `acceler-post-sales:project-reviewer` agent (`agents/project-reviewer.md`) — may run a reference solution's test suite via Bash to verify it, but is read-only by tool restriction (`disallowedTools: Write, Edit, NotebookEdit`) on the guide itself. No visibility into the generating agent's reasoning or other hats' findings.
- Given: the project guide, its reference solution (if reviewable), and the session's stated learning objectives (Curriculum KG / Day-by-Day doc — ask if missing, never invent).
- Output: `{milestone/trap, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Project-specific only:

- "The artifact" = the project guide (and, if the fix touches it, the reference solution). The fixer may touch only these — never the stated objectives or this skill's §1 rubric.
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
| ✅ Approve | All §1 rules PASS, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] A verified reference solution exists and was checked, not assumed
- [ ] Every planted trap has a matching, explicit grading criterion — none are dead weight
- [ ] §1.2's compliance-vs-verified-enforcement distinction was actually applied, not skipped as "too subtle"
- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
