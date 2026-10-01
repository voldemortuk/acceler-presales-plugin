---
name: deck-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler session deck for format correctness and curriculum/objective alignment before it ships — a human-gated fix loop with a dismissed-finding memory, verdicts in PR vocabulary (Approve/Comment/Request Changes). Companion to code-demo-review, mcq-review, assignment-review, and the content-review bundle (cross-artifact coherence, run after every applicable per-type review lands)."
metadata:
  type: reference
---

# Acceler Deck Review — SKILL.md (v1)

Reviews **one deck** in isolation — run this right after `/acceler-post-sales:session-deck` generates or edits one, without needing the rest of the session's bundle. For whether this deck agrees with the notebook/MCQs/assignment for the same session, that's `content-review` (bundle), not here.

---

## 1. Rules

*This section is the tunable rubric — edit it to change the bar. Orchestration mechanics live in §2-3, don't mix them in here.*

### 1.1 Format & Design Correctness
- Matches `live-session-deck/SKILL.md` §2 (palette/typography tokens) and §3 (slide-type library) — flag any slide using colors, fonts, or components outside that system. This is a house-format check, not generic taste. **Clarified 2026-10-01:** §2 now holds three palettes. Check against §2.0, the default. The named alternates (§2.1a Nucleus, §2.1 Hungary) apply only when a human explicitly chose one for this run.
- **Added 2026-09-16: if a `slide-content-plan-day-N.md` exists for this deck, check the deck against it, not just against general taste.** Every Build-movement slide should trace to a plan entry and use that entry's named real component. A slide that drifted to plain text where the plan named a real component (pipeline, pitfall-card, code-block, metrics, query-cards, stack-items, the six `live-session-deck/SKILL.md` §3.17 defines) is a FAIL, this is exactly the failure mode that shipped a 10-of-49-real-components deck on a real test before this check existed.
- No leftover placeholder/lorem-ipsum text — including a "Hook" section heading left as the literal template placeholder word instead of being customized to that slide's actual content (a real, recurring finding in reviewed decks).
- Navigation intact per §4 of that skill: keyboard nav, progress bar, speaker-notes toggle all present and wired.
- All links/citations resolve.
- **No links or viewable copies of instructor-only material (solutions, notebooks, answer keys) hyperlinked or otherwise accessible from student-facing slides.** This is a real, checked-for house standard, not a hypothetical risk — treat any such leak with the same severity as a security finding, not a formatting nitpick.
- Boilerplate slides (standard house slides — cover, agenda, house-rules) are present where the deck type expects them, judged against the run's stated scope per `agent-loops/SKILL.md` §4 (a deliberately short deck isn't failed for slides nobody asked for); a demo slide for each coding/hands-on demo is present in the appropriate position, briefly stating what will be demoed there. **Reconciled 2026-10-01 with `live-session-deck/SKILL.md` §3.20, the two used to disagree:** the learner-facing demo guide link sits on the visible slide as the "Click Here" button on the laptop, that is the real e& layout and is correct, don't flag it. The speaker notes carry the same link again, plus the SME's own direct link to the instructor version (solution notebook, answer key, Colab with outputs). Only that instructor version must never appear on the visible slide, per the rule above. A button pointing at a demo guide that doesn't exist yet is still a FAIL under "all links resolve."
- A quick summary/key-takeaway slide closes each major section.
- **Slide count is sanity-checked against stated session duration** — a real flagged pattern: 150+ slides for a 4-hour class was called out as needing content reduction (without compromising the core objective) or a longer session, not silently shipped. Flag decks whose slide density implies a pace the stated duration can't support.
- No duplicate slides (a real found defect: an exact duplicate slide shipped in a reviewed deck).

### 1.2 Curriculum & Objective Alignment
*Grounded in: Backward Design/UbD, Bloom's Revised Taxonomy.*
- Every stated session objective is taught by at least one slide — a stated objective with no teaching slide is a FAIL. This applies within the run's stated scope, per `agent-loops/SKILL.md` §4: an objective outside the scope a deliberately partial deck was asked to cover is not a FAIL here.
- Extract the Bloom's verb of each objective; the slide(s) teaching it must pitch content at that cognitive level or higher (an "apply X" objective taught only via a bare definition slide is a FAIL).
- Flag disproportionate slide-time on Outer-circle (nice-to-know) material relative to Inner-circle (core-skill) material, given the objectives' stated importance.
- **Orientation and Closing Ceremony decks are out of scope for this skill (corrected 2026-10-01).** Both are PPTX-native, built by editing a real precedent deck in place (`content-generation/SKILL.md` §1e), so §1.1's HTML token check doesn't apply to them, and their job is logistics or wrap-up, so there is no Bloom's-verb objective to check either. Use `orientation-review` or `closing-ceremony-review`, each has its own rubric. Don't review those two deck types here.
- **Single-session masterclasses get a session-scoped version of this rule, not a multi-day one.** Bosch/Nucleus-style one-off 60-90min masterclasses (`B2B Bosch Masterclass`, `B2B Nucleus Masterclass`) have no "day-over-day" arc — "every objective traces to a teaching artifact" still applies, but don't flag the absence of multi-day escalation as if it were a gap; there's no multi-day structure to escalate across.

---

## 2. How this runs

- Launched as the `acceler-post-sales:deck-reviewer` agent (`agents/deck-reviewer.md`) — read-only by tool restriction (`disallowedTools: Write, Edit`), not just by instruction, so it cannot edit the deck even if asked to. It must not see the generating agent's reasoning, another hat's findings, or prior review rounds beyond its own §3 loop.
- Given only: the deck file, the `slide-content-plan-day-N.md` when one exists (§1.1 checks the deck against it), the session's stated learning objectives, and the mode, scope and light brief path when the handoff or the human gives them. **Corrected 2026-10-01:** objectives come from the approved Lesson Plan / Facts Sheet where they exist, otherwise from `Outputs/[Client]/light-brief.md`'s objectives outline, per `agent-loops/SKILL.md` §4. Stop and ask only if none of these exist, never invent them.
- Output: a list of `{slide, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Before surfacing any FAIL, check it against §4 Memories — skip it if a materially identical pattern was already dismissed.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full — round bound, progress/spin checks, maker/checker separation, and the human-approval gate all live there, not here — using the `acceler-post-sales:content-fixer` agent for the apply step. Deck-specific only:

- "The artifact" = the deck file. The fixer may touch only the deck — never the stated objectives or this skill's §1 rubric.
- A human dismissal ("not actually an issue") gets logged to §4 Memories below, not re-surfaced next time.

---

## 4. Memories

*Append-only log of human-dismissed findings. Don't hand-edit §1 to work around a one-off dismissal — log it here instead, so the rule stays intact for the next deck.*

```
- [date] Dismissed: <finding> — <human's stated reason>
```

(empty until first use)

---

## 5. Verdict

| Verdict | Condition |
|---|---|
| ✅ Approve | All §1 rules and the `agent-loops` baseline (§2a, §2a-1, §2b, §4) PASS, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain — ships, stays visible |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

*This verdict is a recommendation, not a ship decision. A human still signs off per `agent-loops/SKILL.md` §2, and per `content-generation/SKILL.md` §6 a per-artifact review is not the final gate, the `content-review` bundle pass still has to run.*

## 6. Checklist
- [ ] Objectives sourced from the approved Lesson Plan / Facts Sheet, or from the light brief's objectives outline where those don't exist (`agent-loops/SKILL.md` §4), not invented
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval
- [ ] No finding exceeded 2 fix rounds before escalating
- [ ] Deck-specific findings cite `live-session-deck/SKILL.md` tokens, not generic taste
