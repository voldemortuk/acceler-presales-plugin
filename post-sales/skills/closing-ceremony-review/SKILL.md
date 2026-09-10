---
name: closing-ceremony-review-skills-acceler-single-artifact-panel
description: "How to review one program Closing Ceremony deck for recap/wrap-up completeness (reflection, key takeaways, next-steps, feedback link) and correct handoff of any embedded assessment slides to mcq-review — backward-looking content, distinct from orientation-review. Human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in a 5-program structural survey, including the finding that Dry Run decks are internal rehearsals of this same deck, not separate content."
metadata:
  type: reference
---

# Acceler Closing Ceremony Review — SKILL.md (v1)

Reviews **one program Closing Ceremony deck**. Confirmed structurally distinct from Orientation (backward-looking wrap-up, frequently embeds a full live assessment as slides), not a shared rubric with two modes — the embedded-assessment handoff requirement here has no equivalent in Orientation.

**Resolved scoping note:** a "Dry Run" version of a Closing deck (confirmed real, e.g. `e& Low Code`'s "Internal Dry Run" 44-slide deck vs. the "Actual Cohorts" 14-slide version) is the *same deck rehearsed internally*, with the full assessment embedded for internal rehearsal instead of linking externally. It is not separate reviewable content — review the live/external version; the Dry Run distinction is an operational/ops-tracking concern (Control Panel), not a content-review concern.

---

## 1. Rules

### 1.1 Wrap-Up Completeness
*Grounded in the confirmed recurring structure across real Closing decks (Bosch, e& AI for Leaders, and others sampled):*
- Reflection prompt present (what participants enjoyed/took away).
- Key Takeaways recap present, specific to what this program actually covered — not a generic template recap disconnected from the real curriculum.
- "Bridging Learning to Application" / concrete next-steps section present.
- Feedback form link present.
- Post-Class/Program Test pointer present, whether that's a link out or embedded content (see §1.2).
- Sign-off/closing slide present.

### 1.2 Embedded-Assessment Handoff
*Grounded in confirmed real pattern: Bosch's and e& Leaders' Closing decks embed 5 full MCQ questions directly as slides.*
- If the Closing deck embeds assessment questions directly as slides (rather than just linking externally), those embedded questions must be handed off to `mcq-review`'s rubric for item-writing validity and objective alignment — **don't duplicate mcq-review's checks here, and don't skip them either.** Flag embedded questions explicitly as "route to mcq-review" rather than evaluating them under this skill's own (non-existent) assessment rubric.
- If the deck only links externally to an assessment, confirm the link/pointer is present and correct — no further handoff needed.

---

## 2. How this runs

- Launched as the `acceler-post-sales:closing-ceremony-reviewer` agent — read-only by tool restriction.
- Given: the Closing deck and, where relevant, access to route any embedded assessment content to `mcq-review`.
- Output: `{section, rule, verdict: PASS|FAIL, evidence, suggested_fix}` — embedded-assessment findings explicitly tagged as "handed off to mcq-review," not evaluated inline.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Closing-Ceremony-specific only:

- "The artifact" = the Closing deck itself (not any embedded assessment content, which is `mcq-review`'s artifact to fix). The fixer may touch only the deck — never the stated program curriculum it's recapping, and never this skill's §1 rubric.
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
| ✅ Approve | All §1 rules PASS (including a clean handoff of any embedded assessment content), directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Key Takeaways recap checked against what this specific program actually covered, not assumed generic
- [ ] Embedded assessment content (if any) explicitly routed to `mcq-review`, not evaluated inline or silently skipped
- [ ] Confirmed this is the live/external deck being reviewed, not mistakenly the internal Dry Run rehearsal version
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
