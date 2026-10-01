---
name: discovery-fit-review-skills-acceler-pipeline-gate
description: "How to gate the Acceler pipeline on discovery completeness before any downstream content gets built, and extract a Discovery Facts Sheet (audience, stated success metric, tool/timeline constraints) that onboarding-form-review and lesson-plan-review check against. Wires the existing acceler-presales discovery-checklist into post-sales rather than re-scoring from scratch. Companion to onboarding-form-review."
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
- **≥80% (Ready to propose):** ✅ Approve, provided the 3-month-success MUST item is answered (see the override below).
- **50-79% (Gaps remain):** 💬 Comment — proceed, but every gap must be explicitly logged, not silently absorbed.
- **<50% (Thin):** 🔴 Request Changes — this blocks the whole downstream pipeline, not just a note. Don't let Lesson Plan or Content Creation start against a Thin discovery brief.
- **MUST-item override, added 2026-10-01 so this section agrees with §5:** if the 3-month-success MUST item (§1.2) is unanswered, the verdict is 🔴 Request Changes regardless of the overall %. It is a human action item per §3 (go back and ask the client), not a fix-loop item. The hard downstream block still belongs to a Thin (<50%) score only, per `content-generation/SKILL.md` §2.

### 1.2 Discovery Facts Sheet
- Extract, verbatim where possible (don't paraphrase into something vaguer): the audience (who/how many/technical level), the stated 3-month success metric/outcome, tool/access constraints, format/timeline constraints, and any regulated-data constraints.
- **Branding, added 2026-09-24.** Every downstream artifact that carries a company name/logo (session deck, Orientation, Closing Ceremony) reads this field rather than asking per artifact, per `content-generation/SKILL.md` §1d. Default `Acceler`, no need to ask for a brand-new engagement. Only actively ask/flag when there's a real signal this might be a continuing relationship from before the rebrand, e.g. a precedent check (`engagement-catalog.md`) turns up this same client's past content already branded PowerUp, in that case ask which one applies to this engagement rather than silently assuming either way.
- **The single most load-bearing fact is "what does success look like 3 months out"** — a brief that scored ≥80% overall but has no real answer to that one MUST item still produces an incomplete Facts Sheet. Flag this specifically; it's the fact `onboarding-form-review` needs most and can't substitute a generic one for.
- This Facts Sheet is a **required input** to `onboarding-form-review` and `lesson-plan-review`. Don't let those reviewers run against invented objectives when a real discovery brief exists to check against.

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
| ✅ Approve | Discovery ≥80%, Facts Sheet complete, including the 3-month-success MUST item |
| 💬 Comment | 50-79%, gaps logged, Facts Sheet has known holes |
| 🔴 Request Changes | <50%, or the 3-month-success MUST item is unanswered regardless of overall % |

*This verdict is a recommendation, not a ship decision. A human still signs off per `agent-loops/SKILL.md` §2, and per `content-generation/SKILL.md` §6 a per-artifact review is fast feedback, not the final gate.*

## 6. Checklist
- [ ] Score came from the existing `discovery-checklist` tool, not a reimplementation
- [ ] Facts Sheet extracted verbatim where possible, not paraphrased vaguer
- [ ] The 3-month-success fact specifically checked, not just the aggregate %
- [ ] Branding field set (Acceler default, or explicitly asked when precedent shows this client's past content was PowerUp-branded), not left unset for downstream skills to guess at
- [ ] Facts Sheet errors (not score gaps) are the only thing routed through the fix loop
