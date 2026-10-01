---
name: session-recap-review-skills-acceler-single-artifact-panel
description: "How to review one learner-facing day-N recap for content fidelity (traces to the actual session), structural correctness (section order, last-day branching, correct Q&A component per type), and house prose style — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Grounded in session-recap's own build spec. Companion to impact-report-review (the stakeholder-facing sibling)."
metadata:
  type: reference
---

# Acceler Session Recap Review — SKILL.md (v1)

Reviews **one learner-facing day-N recap** (the artifact `session-recap` generates). Sits after Stage 9 (Delivery) in the pipeline — a post-delivery review, distinct from `content-review`'s pre-ship bundle.

---

## 1. Rules

### 1.1 Content Fidelity
- Topics/parts covered must trace to the actual session deck and/or instructor notes for that day — no invented content standing in for what was actually taught.
- Parked/rushed questions (`.qa` section) must be real, attributed, sourced from the actual chat log/transcript — never an invented generic FAQ.

### 1.2 Structural Correctness
*Grounded in `session-recap/SKILL.md` §2.1-2.4.*
- Section order matches the standard order; sections that don't apply that day are skipped, not force-included.
- Instructor bio (`#instructor`) appears only on a first-introduction or handoff day — never both a handoff note AND a full bio reintroduction for a day where the instructor is unchanged.
- Quiz vs. discussion-question format matches whether a quiz actually ran that day (`.q` scored format only if real, `.pk` discussion-only otherwise) — using the scored format for a day with no real quiz is a FAIL.
- Last-day branching is correct: `#wrapup` + `#assessment` only on the actual last day; `#before` (Before Day N+1) only mid-program. Getting this backwards on the last day is a real, checkable defect.
- The right Q&A component is used for the right content per §2.4 (`.qa` for parked/rushed questions, `.q` only for a real scored quiz, `.pk` for everything else) — component misuse is a FAIL, not a style nitpick.

### 1.3 Prose Style
- The prose bans are the ones in `agent-loops/SKILL.md` §2a-1, applied as written there (corrected 2026-10-01: the copy that used to sit here had dropped one of them). Direct quotes and `<code>` content are exempt.
- One recap-specific check on top: grep the file for `&mdash;` and confirm zero remain outside `<style>`/`<script>` and direct quotes.

---

## 2. How this runs

- Launched as the `acceler-post-sales:session-recap-reviewer` agent — read-only by tool restriction.
- Given: the recap HTML, the session's actual deck/notes, and the chat log/transcript for parked-question verification.
- Output: `{section, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Session-recap-specific only:

- "The artifact" = the recap HTML. The fixer may touch only the recap — never the source deck/transcript it's built from, and never this skill's §1 rubric.
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
| ✅ Approve | All §1 rules and the `agent-loops` baseline (§2a, §2a-1, §2b, §4) PASS, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

*This verdict is a recommendation, not a ship decision. A human still signs off per `agent-loops/SKILL.md` §2, and per `content-generation/SKILL.md` §6 a per-artifact review is fast feedback, not the final gate.*

## 6. Checklist
- [ ] Content traced against the real session deck/notes, not assumed accurate
- [ ] Parked questions verified against the actual chat log/transcript
- [ ] Last-day branching logic checked explicitly — this is the easiest thing to get backwards
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
