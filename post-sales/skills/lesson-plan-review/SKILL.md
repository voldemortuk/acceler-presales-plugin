---
name: lesson-plan-review-skills-acceler-single-artifact-panel
description: "How to review one Lesson Plan (the internal, minute-level facilitator table: Topic/Objective/Subtopic/Time/Flow/Demo/Tools per row) for structural completeness, timing-math consistency, Bloom's-verb objective alignment, and cross-artifact tool consistency — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in real Lesson Plan xlsx samples. Genuinely distinct from a client-facing Day-by-Day proposal doc and from a session deck."
metadata:
  type: reference
---

# Acceler Lesson Plan Review — SKILL.md (v1)

Reviews **one Lesson Plan** — the internal, granular facilitator planning table that precedes content creation, not the client-facing Day-by-Day proposal section and not the session deck itself. Confirmed real structure from sampled xlsx files (e& Tech Teams, 2 B2C Agentic AI modules): one row per curriculum segment, columns `Topic | Learning Objective | Subtopic | Estimated Time | Flow of Examples & Topics | Live Demo/Coding Demo | Libraries/Tools`, split across day-tabs for multi-day programs.

---

## 1. Rules

### 1.1 Structural Completeness
- The 7-column structure is present and populated per row, **with one confirmed legitimate exception**: break/lunch rows (Topic = a time range only, everything else blank) and AMA rows (Subtopic = `-`, Flow blank) are valid as-is — don't flag these as incomplete rows, that's a false positive.
- **Timing format must be internally consistent** — don't mix clock-ranges ("10:30–11:00") and raw durations ("00:30:00") across days of the same plan; pick one convention per document.
- **Row durations for a given day must sum to that day's stated/expected session length.** This is a real, previously-unchecked gap: nothing currently verifies the math. A day whose rows sum to less or more than the stated total is a FAIL.
- **Every module's plan should cover pre-class, live-class, and post-class topics as distinct, identifiable segments** — grounded in Acceler's own real module-creation checklist, which separately asks "does the lesson plan include the pre-class topics," "live-class topics," and "post-class topics." A plan that only covers live-class content, with no stated pre-class prep or post-class follow-up, is incomplete against the house standard even if the live-class rows themselves are well-formed — flag it as a completeness gap, not assume pre/post simply don't apply.

### 1.1a Review-Gate Status (process check, house checklist)
- Confirm, don't assume: has this plan been reviewed internally AND by an SME (two distinct checkpoints, per `agent-loops/SKILL.md` §2's dual-gate note), and have review comments actually been addressed before this is treated as finalized? A plan presented for content-creation sign-off with no record of either review pass is a FAIL — content creation shouldn't start against an unreviewed plan.

### 1.1b Cross-Day Demo Load Balancing
*Grounded in a real threaded review comment on an actual e& Lesson Plan: "Demos have to be lightweight as there are dedicated sections for demos in day 3," with the resolution planning one lightweight demo after a concept was introduced and pointing the heavier build to its own dedicated day-3 slot.*
- Where a program has a day/days explicitly dedicated to hands-on building, demo weight on the introductory/concept days should stay light — check for a demo on an earlier day that's actually doing the heavy-build work a later dedicated day is supposed to own. This is a cross-day pacing defect, not just a per-row one; catching it requires looking at demo weight across the whole plan, not row-by-row in isolation.

### 1.2 Curriculum & Objective Alignment
*Reuses the same Bloom's-verb-match mechanism as `deck-review`/`code-demo-review` §1.2, applied at the Topic-row level instead of the slide/task level.*
- Extract the Bloom's verb from each row's Learning Objective; the row's Live Demo/Coding Demo content must operate at that level or higher (a demo exceeding the stated objective's level is fine, not a defect — e.g. an "Understand RAG" objective paired with a hands-on chunking/embedding build is a legitimate stretch, not a FAIL).
- Every stated objective should be reflected in that row's Flow/Demo content — an objective with nothing in Flow or Demo backing it up ("orphaned objective") is a FAIL.

### 1.3 Cross-Artifact Tool Consistency
- The Libraries/Tools column for a given row should match what that day's actual code-demo notebook or hands-on guide uses. This is a local heads-up, cheap to catch early here — the authoritative cross-check for the finished session bundle still runs in `content-review`, don't block solely on this skill's own check.

---

## 2. How this runs

- Launched as the `acceler-post-sales:lesson-plan-reviewer` agent — read-only by tool restriction.
- Given: the Lesson Plan file and, where available, that day's actual code-demo/hands-on-guide artifacts for the §1.3 cross-check. If those aren't available yet, run §1.1-1.2 only and say plainly that §1.3 couldn't run.
- Output: `{row, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Lesson-Plan-specific only:

- "The artifact" = the Lesson Plan file. The fixer may touch only the plan — never the session's stated objectives (which the plan itself partly defines — don't let the fixer weaken an objective just to make a demo-alignment finding disappear) or this skill's §1 rubric.
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
| 💬 Comment | Only dismissed/subjective findings remain, or §1.3 couldn't run for lack of same-day artifacts |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Break/lunch/AMA rows correctly recognized as legitimate, not flagged as incomplete
- [ ] Row-duration sums checked against the stated day length — this is the check most likely to be skipped
- [ ] Every objective traced to Flow/Demo content — no orphaned objectives
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
