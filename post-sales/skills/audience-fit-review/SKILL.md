---
name: audience-fit-review-skills-acceler-session-bundle-hat
description: "How to review a session's content bundle for calibration against the ACTUAL audience (aggregated onboarding form responses: proficiency, role, stated concerns) and the program's stated expectations (Discovery Facts Sheet) — distinct from coherence-reviewer, which checks internal cross-artifact consistency, not external fit to real people. Human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in real editorial feedback on an actual Lesson Plan. Runs inside content-review alongside coherence-reviewer."
metadata:
  type: reference
---

# Acceler Audience Fit Review — SKILL.md (v1)

**What this checks that nothing else in this family does:** every other reviewer checks an artifact against its own stated objectives, or against other artifacts in the same bundle. None of them check the content against the *actual humans who will sit through it* — their real proficiency, their real role, what they actually said they're worried about. That's this skill's whole job.

Grounded in real threaded review comments on an actual e& Lesson Plan (not hypothetical): *"Cut the basic fundamentals and focus on numpy and Pandas. Focus on practical side"* and *"Add more examples and details relevant to engineers"* — both are audience-fit corrections made by a human reviewer after the fact. This skill exists to catch that class of finding before a human has to.

---

## 1. Rules

### 1.1 Depth Calibration
*Grounded in the real "cut the basic fundamentals" comment — evidence this is a recurring, real defect, not hypothetical.*
- Cross-check content depth/pacing against the cohort's **aggregate self-rated proficiency** from the onboarding form (`onboarding-form-review` §1.3's familiarity matrix, aggregated across respondents). Content spending disproportionate time on fundamentals a cohort has predominantly rated itself Intermediate-or-above on is a FAIL — that time belongs to something the cohort actually needs.
- The inverse also applies: content assuming proficiency a cohort predominantly rated Never/Beginner on, with no on-ramp, is equally a FAIL.

### 1.2 Example & Context Relevance
*Grounded in the real "add examples relevant to engineers" comment.*
- Examples, case studies, and demo scenarios should be recognizable to the audience's actual role/domain (from onboarding form identity/team-function fields) — generic or domain-mismatched examples (e.g. retail case studies for a fintech engineering cohort) are a FAIL, not a stylistic nitpick, when a same-effort domain-relevant example was available.

### 1.3 Program-Expectations Fidelity
*The session-bundle-level counterpart to `agent-loops` §2b — that section is a per-artifact check; this is the check that looks at the whole bundle's scope/pacing together against what was promised.*
- The bundle's actual scope, format, and pacing should be traceable to the Discovery Facts Sheet's stated constraints (format/duration, stated tools, any regulated-data constraint) — a bundle that quietly drifted from what was scoped (e.g. built for tools the client's IT blocks, per the Facts Sheet) is a FAIL.

### 1.4 Concern-Responsiveness *(when the onboarding form captured stated concerns)*
- If onboarding responses show a common stated concern (e.g. multiple learners flagging security/secrets, or over-reliance on AI output), check that the content actually addresses it somewhere identifiable — not necessarily a dedicated section, but not silently absent either. A cohort that repeatedly flagged a concern the content never touches is a FAIL.

---

## 2. How this runs

- Launched as the `acceler-post-sales:audience-fit-reviewer` agent — read-only by tool restriction.
- Given: the session's content bundle (whichever artifacts `content-review` is reviewing), the aggregated onboarding form responses for this cohort, and the Discovery Facts Sheet. If either the onboarding responses or the Facts Sheet is missing, run whichever of §1.1-1.4 that input supports and say plainly which checks couldn't run — never invent an audience profile to check against.
- Output: `{artifact/section, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Audience-fit-specific only:

- "The artifact" = whichever bundle artifact the fix actually touches. The fixer may touch only that artifact — never the onboarding response data, the Discovery Facts Sheet, or this skill's §1 rubric.
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
| 💬 Comment | Only dismissed/subjective findings remain, or a rule couldn't run for lack of onboarding data/Facts Sheet |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Onboarding responses and Discovery Facts Sheet checked for and used where available; absence explicitly flagged where not
- [ ] Depth-calibration finding backed by actual aggregate proficiency data, not a guess at "what engineers probably know"
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
