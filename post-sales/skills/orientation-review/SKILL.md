---
name: orientation-review-skills-acceler-single-artifact-panel
description: "How to review one program Orientation deck for logistics-coverage completeness (schedule, expectations, tool-access pointers, program overview) — forward-looking content, not Bloom's-taxonomy teaching content. Human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in a 5-program structural survey. Distinct from closing-ceremony-review (backward-looking, often embeds live assessment content)."
metadata:
  type: reference
---

# Acceler Orientation Review — SKILL.md (v1)

Reviews **one program Orientation deck**. Confirmed via a 5-program survey (e& AI for Leaders, Bosch, Nucleus, Deloitte, Cornerstone) that Orientation has a consistent recurring structure across every program sampled — this is a completeness check against that real structure, not a Bloom's-taxonomy content review (`deck-review`'s §1.2 explicitly doesn't apply here, per its own carve-out).

---

## 1. Rules

### 1.1 Logistics-Coverage Completeness
*Grounded in the confirmed recurring structure across 5 real Orientation decks:*
- Company/program credibility framing present (who's delivering this, why they're credible).
- Instructor/speaker introduction present (bio, career highlights, specializations) for every instructor who'll appear.
- Program overview present: topic, duration, format, target audience, session flow, and — critically — **stated final outcomes for participants** (confirmed real section: `PowerUp _ Bosch Masterclass Orientation.pptx`). A deck missing this is a FAIL — it's the one section that ties Orientation back to what the client was actually promised.
- Time-blocked schedule/agenda present (hour-by-hour or day-by-day, not just a vague "Day 1 / Day 2" label).
- "Expectations From Learners" present (focus, participation, doubt-resolution norms) — confirmed present in some programs, absent in others sampled; treat absence as a real completeness gap, not house-style variance.
- Pointer to the pre-course assessment, if one exists for this program.

### 1.2 Curriculum & Objective Alignment — does not apply
- Orientation content is expectations/logistics-setting, not teaching content. Don't force `deck-review` §1.2's Bloom's-verb check onto this deck type — there's no teaching objective to extract a verb from.

### 1.3 House Branding & Format
- Same design-token/format discipline as `deck-review` §1.1 (matches `live-session-deck` tokens, no placeholder text, navigation intact) — this part is shared with every other deck type in the family.

---

## 2. How this runs

- Launched as the `acceler-post-sales:orientation-reviewer` agent — read-only by tool restriction.
- Given: the Orientation deck and, where available, the program's actual schedule/format details to check the agenda against.
- Output: `{section, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Orientation-specific only:

- "The artifact" = the Orientation deck. The fixer may touch only the deck — never the program's actual schedule/format facts it's supposed to reflect, and never this skill's §1 rubric.
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
- [ ] "Final outcomes for participants" section present and specific, not boilerplate
- [ ] Schedule is genuinely time-blocked, not a vague day label
- [ ] §1.2 (Bloom's alignment) correctly NOT applied to this deck type
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
