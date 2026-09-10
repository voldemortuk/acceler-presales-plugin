---
name: discovery-fit-review-skills-acceler-pipeline-gate
description: "How to gate the Acceler pipeline on discovery completeness before any downstream content gets built, and extract a Discovery Facts Sheet (audience, stated success metric, tool/timeline constraints) that onboarding-form-review and future lesson-plan review check against. Wires the existing acceler-presales discovery-checklist into post-sales rather than re-scoring from scratch. Companion to onboarding-form-review."
metadata:
  type: reference
---

# Acceler Discovery Fit Review — SKILL.md (v1)

Sits at the front of the pipeline, before any content exists — distinct from `content-review`'s bundle (which reviews Stage 6-7 content artifacts). This skill does two things: gate on discovery completeness, and produce the one reference document every downstream form/lesson-plan reviewer needs — what discovery actually established about this client.

---

## 1. Rules

### 1.1 Discovery Completeness Gate
*Reuses, does not reimplement, `acceler-presales:discovery-checklist` (33 questions, 6 sections, %-scored, existing tool).*
- Score the discovery brief via that existing checklist. Don't build a second scoring mechanism.
- **≥80% (Ready to propose):** ✅ Approve.
- **50-79% (Gaps remain):** 💬 Comment — proceed, but every gap must be explicitly logged, not silently absorbed.
- **<50% (Thin):** 🔴 Request Changes — this blocks the whole downstream pipeline, not just a note. Don't let Lesson Plan or Content Creation start against a Thin discovery brief.

### 1.2 Discovery Facts Sheet
- Extract, verbatim where possible (don't paraphrase into something vaguer): the audience (who/how many/technical level), the stated 3-month success metric/outcome, tool/access constraints, format/timeline constraints, and any regulated-data constraints.
- **The single most load-bearing fact is "what does success look like 3 months out"** — a brief that scored ≥80% overall but has no real answer to that one MUST item still produces an incomplete Facts Sheet. Flag this specifically; it's the fact `onboarding-form-review` needs most and can't substitute a generic one for.
- This Facts Sheet is a **required input** to `onboarding-form-review` and, once it exists, a Lesson Plan reviewer. Don't let those reviewers run against invented objectives when a real discovery brief exists to check against.

---

## 2. How this runs

- Launched as the `acceler-post-sales:discovery-fit-reviewer` agent — read-only by tool restriction.
- Given: the discovery brief/notes and access to the `discovery-checklist` skill's scoring logic.
- Output: the % score + verdict, plus the structured Facts Sheet, plus a list of any unanswered MUST items.

---

## 3. Fix & re-verify loop — different shape than the content reviewers

**A low discovery score is not a text defect an agent can fix.** Unlike a deck slide or an MCQ item, a discovery brief's gaps get closed by a human going back to the client and asking, not by an editor rewriting a paragraph. So this skill's "fix loop" only ever applies to one thing: an extraction error in the Facts Sheet itself (the reviewer mis-transcribed a fact that's actually present in the brief). That follows `skills/agent-loops/SKILL.md`'s normal mechanics via `acceler-post-sales:content-fixer`.

A missing MUST item or a sub-80% score is never "fixed" this way — it escalates directly to a human action item ("go back and ask the client what success looks like in 3 months"), not to the fix loop.

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
| ✅ Approve | Discovery ≥80%, Facts Sheet complete |
| 💬 Comment | 50-79%, gaps logged, Facts Sheet has known holes |
| 🔴 Request Changes | <50%, or the 3-month-success MUST item is unanswered regardless of overall % |

## 6. Checklist
- [ ] Score came from the existing `discovery-checklist` tool, not a reimplementation
- [ ] Facts Sheet extracted verbatim where possible, not paraphrased vaguer
- [ ] The 3-month-success fact specifically checked, not just the aggregate %
- [ ] Facts Sheet errors (not score gaps) are the only thing routed through the fix loop
