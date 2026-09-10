---
name: mcq-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler MCQ set for item-writing validity (NBME rules) and curriculum/objective alignment before it ships — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Companion to deck-review, code-demo-review, assignment-review, and the content-review bundle."
metadata:
  type: reference
---

# Acceler MCQ Review — SKILL.md (v1)

Reviews **one MCQ set** in isolation, per-item. Kept separate from `assignment-review` — item-writing defects (distractor quality, cueing) are a different failure mode from rubric/answer-key defects in open-ended assignments, and conflating them dilutes both rubrics.

---

## 1. Rules

### 1.1 Item-Writing Validity
*Grounded in: the NBME Item-Writing Guide, cross-checked against a real Acceler MCQ set (`MCQ - Building Applications with LLMs.docx`) to confirm these defects actually occur in-house, not just in theory. Check every item for these binary-flaggable defects:*
- **Implausible distractors** — a distractor no one who skimmed the material would plausibly pick.
- **Malformed negative stem** — "EXCEPT"/"NOT" used without required emphasis/formatting.
- **Grammatical cueing** — a distractor doesn't grammatically match the stem, tipping the answer.
- **Length/specificity giveaway** — the correct answer is conspicuously longer or more detailed than distractors. *Worked example of the near-miss version of this: two options sharing nearly identical text, differing only in a trailing clause — a test-taker who spots the shared stem can eliminate one without understanding the content. This exact pattern occurs in the reference set's Q11 (options B and C).*
- **Ambiguous best answer** — more than one option is defensibly correct **for an item the stem frames as single-best-answer**. Check the stem's intended format first: some Acceler assessments are legitimately multi-select (e.g. `Correct: B, D`), confirmed in ETS and e& No-Code Pre-Program Assessments. Applying this rule to a genuinely multi-select item is a false positive — detect the format from the stem/instructions before flagging.
- **Compound/K-type claim disguised as a single item** — a Yes/No or true/false item whose stem bundles multiple independent claims (e.g. "does X do A, B, *and* C — yes/no") is a FAIL even if each claim is individually true, because a learner confident on 2 of 3 claims still can't answer with confidence. This is a real, recurring pattern in-house — flag it even though it's subtler than a wrong-answer defect.
- **Yes/No-statement items get the same rules as 4-option items** — this format is common in Acceler MCQs (not just the standard stem+4-options shape). A Yes/No item still needs a single unambiguous claim, must test real understanding rather than trivia, and is exactly where the compound-claim defect above tends to hide.
- **Explanation completeness** — the solution explanation must address why each *wrong* option is wrong, not only why the correct option is right. One reference set (`MCQ - Building Applications with LLMs.docx`) does this well; three others sampled (Bosch and Nucleus Masterclass Pre-Class Assessments, the e& Post-Course Assessment) have **zero explanations at all**, just a bare answer letter — confirming this is a real, widespread in-house gap, not a one-off. For new content this is a FAIL, not a nice-to-have; on legacy content flag it but expect volume, don't treat it as a surprise defect.
- **Answer-key format consistency** — the correct-answer line should consistently show the option letter plus its text (e.g. `B) Enhancing content filtering systems`), not a bare-text bullet with no letter. The reference set is inconsistent on this; don't propagate that inconsistency into new sets.
- **Topic tag, when the house convention uses one** — Bosch/Nucleus Masterclass assessments tag each question with its sub-topic (`MCP`, `A2A`) right after the answer. Where this tag exists, use it directly for the §1.2 alignment check instead of inferring the mapping; where it's absent, that's a minor format gap to note, not a hard FAIL.
- **Reused/templated assessments still get the alignment check** — Bosch and Nucleus Masterclasses used the *verbatim-identical* assessment for two different client sessions. Reuse across clients/cohorts is normal practice, but it does not exempt the set from §1.2 — verify it against *this* session's actual deck content, not against "it was used before."
- **Stated difficulty label, when present** — some assessments carry a difficulty label in the title itself (e.g. "Pre-Class Assessment (Hard)"). Where present, check the item content actually matches that stated level, and that pre- vs. post-assessment difficulty is sensibly calibrated relative to each other (this is the concrete, checkable seed of the Pacing & Difficulty lens once that's formally added).

### 1.2 Curriculum & Objective Alignment
- Every question maps to a stated session objective — a question testing untaught content is a FAIL.
- The question's Bloom's level matches or exceeds the objective's stated level (a "recall the definition" question testing an "apply X" objective is a FAIL).

---

## 2. How this runs

- Launched as the `acceler-post-sales:mcq-reviewer` agent (`agents/mcq-reviewer.md`) — read-only by tool restriction (`disallowedTools: Write, Edit`). No visibility into the generating agent's reasoning or other hats' findings.
- Given: the MCQ set and the session's stated learning objectives (Curriculum KG / Day-by-Day doc — ask if missing, never invent).
- Output: `{question #, rule, verdict: PASS|FAIL, evidence, suggested_fix}`, evaluated per-item, not just per-set.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. MCQ-specific only:

- "The artifact" = the individual question item. The fixer may touch only the item — never the stated objective or this skill's §1 rubric (don't let it "fix" an untaught-content FAIL by loosening what counts as taught).
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
| ✅ Approve | All §1 rules PASS on every item, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] Every item checked individually, not just the set as a whole
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval
- [ ] No finding exceeded 2 fix rounds before escalating
