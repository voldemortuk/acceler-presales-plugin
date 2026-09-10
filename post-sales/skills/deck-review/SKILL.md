---
name: deck-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler session deck for format correctness and curriculum/objective alignment before it ships — a human-gated fix loop with a dismissed-finding memory, verdicts in PR vocabulary (Approve/Comment/Request Changes). Companion to code-demo-review, mcq-review, assignment-review, and the content-review bundle (cross-artifact coherence, run after all four land)."
metadata:
  type: reference
---

# Acceler Deck Review — SKILL.md (v1)

Reviews **one deck** in isolation — run this right after `/acceler-post-sales:session-deck` generates or edits one, without needing the rest of the session's bundle. For whether this deck agrees with the notebook/MCQs/assignment for the same session, that's `content-review` (bundle), not here.

---

## 1. Rules

*This section is the tunable rubric — edit it to change the bar. Orchestration mechanics live in §2-3, don't mix them in here.*

### 1.1 Format & Design Correctness
- Matches `live-session-deck/SKILL.md` §2 (palette/typography tokens) and §3 (slide-type library) — flag any slide using colors, fonts, or components outside that system. This is a house-format check, not generic taste.
- No leftover placeholder/lorem-ipsum text — including a "Hook" section heading left as the literal template placeholder word instead of being customized to that slide's actual content (a real, recurring finding in reviewed decks).
- Navigation intact per §4 of that skill: keyboard nav, progress bar, speaker-notes toggle all present and wired.
- All links/citations resolve.
- **No links or viewable copies of instructor-only material (solutions, notebooks, answer keys) hyperlinked or otherwise accessible from student-facing slides.** This is a real, checked-for house standard, not a hypothetical risk — treat any such leak with the same severity as a security finding, not a formatting nitpick.
- Boilerplate slides (standard house slides — cover, agenda, house-rules) are present where the deck type expects them; a placeholder slide for each coding/hands-on demo is present in the appropriate position, briefly stating what will be demoed there, with the Colab/notebook link in the speaker notes (not on the visible slide).
- A quick summary/key-takeaway slide closes each major section.
- **Slide count is sanity-checked against stated session duration** — a real flagged pattern: 150+ slides for a 4-hour class was called out as needing content reduction (without compromising the core objective) or a longer session, not silently shipped. Flag decks whose slide density implies a pace the stated duration can't support.
- No duplicate slides (a real found defect: an exact duplicate slide shipped in a reviewed deck).

### 1.2 Curriculum & Objective Alignment
*Grounded in: Backward Design/UbD, Bloom's Revised Taxonomy.*
- Every stated session objective is taught by at least one slide — a stated objective with no teaching slide is a FAIL.
- Extract the Bloom's verb of each objective; the slide(s) teaching it must pitch content at that cognitive level or higher (an "apply X" objective taught only via a bare definition slide is a FAIL).
- Flag disproportionate slide-time on Outer-circle (nice-to-know) material relative to Inner-circle (core-skill) material, given the objectives' stated importance.
- **This rule doesn't apply as written to Orientation and Closing Ceremony decks** — both are real, recurring deck types (every program sampled has one of each: e.g. `PowerUp _ e& Pilot AI for Leaders _ Orientation.pptx`, `Bosch Closing Ceremony.pptx`). Their "objective" is expectations/logistics (what to expect, how to get help, next steps) or wrap-up/certification, not a Bloom's-taxonomy teaching objective — don't force-fit this rule onto them; check instead that they cover the logistics they're supposed to (schedule, support channels, access info for Orientation; recap, certificate/next-steps, feedback ask for Closing) and skip the Bloom's-verb check entirely for these two deck types.
- **Single-session masterclasses get a session-scoped version of this rule, not a multi-day one.** Bosch/Nucleus-style one-off 60-90min masterclasses (`B2B Bosch Masterclass`, `B2B Nucleus Masterclass`) have no "day-over-day" arc — "every objective traces to a teaching artifact" still applies, but don't flag the absence of multi-day escalation as if it were a gap; there's no multi-day structure to escalate across.

---

## 2. How this runs

- Launched as the `acceler-post-sales:deck-reviewer` agent (`agents/deck-reviewer.md`) — read-only by tool restriction (`disallowedTools: Write, Edit`), not just by instruction, so it cannot edit the deck even if asked to. It must not see the generating agent's reasoning, another hat's findings, or prior review rounds beyond its own §3 loop.
- Given only: the deck file and the session's stated learning objectives (sourced from the Curriculum KG or the program's Day-by-Day doc — if objectives can't be found, stop and ask; never invent them).
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
| ✅ Approve | All §1 rules PASS, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain — ships, stays visible |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval
- [ ] No finding exceeded 2 fix rounds before escalating
- [ ] Deck-specific findings cite `live-session-deck/SKILL.md` tokens, not generic taste
