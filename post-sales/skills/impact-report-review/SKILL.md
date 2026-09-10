---
name: impact-report-review-skills-acceler-single-artifact-panel
description: "How to review one stakeholder-facing day-N impact report for attribution integrity (real quotes attributed to real named learners must actually be real), internal numeric consistency, and padding discipline — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in session-recap's stakeholder-report build spec. Companion to session-recap-review (the learner-facing sibling)."
metadata:
  type: reference
---

# Acceler Impact Report Review — SKILL.md (v1)

Reviews **one stakeholder-facing day-N recap report** (`session-recap`'s stakeholder artifact). This is the one review skill in the family where the accuracy stakes are highest by nature of the content: it attributes real quotes and behavior notes to real, named learners, sent to the paying client. A fabricated attribution here is not a quality nitpick, it's a factual-accuracy defect about a real person.

---

## 1. Rules

### 1.1 Attribution Integrity (the load-bearing rule)
*Grounded in `session-recap/SKILL.md` §3.2-3.3: "Real quotes only — pull from the actual chat export/transcript, never paraphrase into a quote."*
- Every quote in "The Evidence Behind the Tiers" must be a real, verifiable quote from the actual chat export or transcript, attributed correctly by name and source (`Chat`/`Transcript`). A quote that reads plausible but can't be traced to the actual source material is a FAIL, treated with the same severity as a security finding — this is about a real person's words, not house style.
- Every present learner, in every tier including Beginners, gets a specific, evidence-based note — "no questions all day" is a legitimate note for a quiet Beginner; a generic "actively engaged" with nothing behind it is a FAIL, because it defeats the report's actual purpose of proving engagement with evidence.

### 1.2 Internal Consistency
- Per-tier metric breakdowns (§3.1 point 5) must reconcile to the cohort-wide KPI figures already shown at the top of the report — a per-tier sum or average that doesn't reconcile is a FAIL, not a rounding footnote.
- The tier-assignment rule must be explicitly stated in the section note (§3.1 point 2) AND actually followed — spot-check a sample of tier assignments against that stated rule; assignment by tenure/seniority instead of that day's actual behavior is a FAIL.
- Any respondent whose feedback-form link and actual role diverge (§3.1 point 6) must be noted, not silently absorbed into the wrong bucket.

### 1.3 Padding Discipline
- Optional sections (Themes, Quotes, companion Assessment card) are included only if the source deck defines them and that day's actual data supports them (§3.1 point 8) — a report padding in a section with no real data behind it is a FAIL.

---

## 2. How this runs

- Launched as the `acceler-post-sales:impact-report-reviewer` agent — read-only by tool restriction.
- Given: the report HTML, the actual chat export/transcript (for quote verification), and the KPI source numbers.
- Output: `{section/quote, rule, verdict: PASS|FAIL, evidence, suggested_fix}`. §1.1 findings should be flagged as high-severity explicitly in the output, not just listed alongside routine format findings.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Impact-report-specific only:

- "The artifact" = the report HTML. The fixer may touch only the report — never the underlying chat/transcript source, and never this skill's §1 rubric.
- **A §1.1 attribution FAIL should not go through 2 rounds of blind auto-fix the way a format defect does** — a fabricated quote about a real person needs the human to see and personally verify the correction (find the real quote or remove the claim), not a fixer agent guessing a replacement. Escalate §1.1 findings to the human on the first occurrence, don't spend a fix-loop round on them.
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
| 🔴 Request Changes | Any §1.1 attribution finding, or any other FAIL that didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Every tier-evidence quote traced to real chat/transcript source, not just plausible-sounding
- [ ] Every present learner in every tier has a specific, non-generic note
- [ ] Per-tier numbers reconciled against cohort-wide KPIs
- [ ] §1.1 findings escalated to the human immediately, not run through the normal 2-round fix loop
- [ ] Every finding checked against §4 Memories before being surfaced
