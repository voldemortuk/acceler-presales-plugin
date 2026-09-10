---
name: assignment-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler assignment for rubric/answer-key correctness, curriculum/objective alignment, and data-boundary safety (when code-based) before it ships — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Kept separate from mcq-review: assignments are typically open-ended/rubric-graded, not multiple-choice, so the defect modes differ. Companion to deck-review, code-demo-review, and the content-review bundle."
metadata:
  type: reference
---

# Acceler Assignment Review — SKILL.md (v1)

Reviews **one assignment** in isolation. Whether it *reinforces* the session's code-demo is checked authoritatively at the bundle level (`content-review`) — this skill only checks local correctness, but flags the reinforcement question too since it's cheap to catch early (see §1.2).

---

## 1. Rules

### 1.1 Rubric & Answer-Key Correctness
- If auto-graded: the answer key is internally correct given the assignment as written.
- Grading criteria are unambiguous enough that two reasonable graders would reach the same score — a weighted rubric table (criteria + % weights, e.g. Functionality 30% / Technical Implementation 25% / UX 20% / Setup & Reproducibility 10% / Documentation 10%, confirmed real house format from an e& capstone gradesheet template) is the target shape; a rubric without weights or without stated criteria is a FAIL, not just thin.
- **Starter-kit / partial-scaffold assignments must state the split explicitly.** A real house pattern (confirmed: e& Low Code capstone template) gives learners a partially-built starter (e.g. "this gives you ~40% of the work") and grades the remainder. If a starter kit is provided, the assignment must explicitly state what fraction/which parts are pre-built vs. what's graded — an assignment that hands over a starter with no stated boundary between "given" and "your work" is a FAIL, since learners and graders can't tell what's actually being assessed.
- A subjective/open-ended item's guidance may be either a full model answer or a "tips to help you answer" scaffold (both are legitimate, confirmed house patterns) — but whichever form is used, grading criteria must still be concrete enough to score consistently; "tips" alone with no scoring criteria at all is a FAIL, tips *plus* a marks breakdown (e.g. Problem Identification 2 marks / Considerations 2 marks / Metrics 1 mark) is not.

### 1.2 Curriculum & Objective Alignment
- Assignment exercises its stated objective(s) at the right Bloom's level — assignments are typically higher-order than MCQs (Apply/Analyze/Create); flag one that only demands Remember/Understand-level work.
- If a companion code-demo exists for this session, this assignment should build on the same tool/technique, not a disconnected task. (Local heads-up only — the authoritative cross-artifact check runs in `content-review`; don't block here on a coherence call this skill can't fully verify alone.)

### 1.3 Security & Data-Boundary *(conditional: assignment is code-based)*
- No real API keys, credentials, or secret-shaped strings.
- No real customer/PII data in sample datasets — must read as obviously synthetic.
- No license-incompatible copied code blocks.

### 1.4 Setup & Resourcing Clarity *(conditional: assignment is code-based)*
*Grounded in real reviewer feedback on a capstone assignment brief.*
- Provided data files are described (format, and whether they contain text/tables/images — not just handed over unexplained).
- Required languages/libraries/prerequisites are stated up front, not left for the learner to discover by trial and error.
- Execution environment is specified (local vs. hosted notebook, and any hardware/spec expectations) — an assignment silent on this is a FAIL if the work has real compute requirements.
- Where the assignment is open-ended by design (learners expected to research), a few starting reference links/resources are still provided — full silence on where to start is a FAIL, not a feature of open-endedness.

---

## 2. How this runs

- Launched as the `acceler-post-sales:assignment-reviewer` agent (`agents/assignment-reviewer.md`) — read-only by tool restriction (`disallowedTools: Write, Edit`). No visibility into the generating agent's reasoning or other hats' findings.
- Given: the assignment file(s) and the session's stated learning objectives (Curriculum KG / Day-by-Day doc — ask if missing, never invent).
- Output: `{item, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Assignment-specific only:

- "The artifact" = the assignment file. The fixer may touch only the artifact — never the stated objective or this skill's §1 rubric.
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
| 💬 Comment | Only dismissed/subjective findings remain, or the local coherence heads-up (§1.2) needs the bundle-level check to confirm |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] §1.3 only runs if the assignment is code-based — don't flag a written-answer assignment for missing security checks
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval
- [ ] No finding exceeded 2 fix rounds before escalating
