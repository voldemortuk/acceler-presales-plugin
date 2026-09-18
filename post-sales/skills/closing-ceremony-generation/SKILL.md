---
name: closing-ceremony-generation-skills-acceler-wrap-up-deck
description: "Generates one program Closing Ceremony deck, backward-looking wrap-up content, following the confirmed recurring structure across a real 5-program survey. If assessment questions get embedded as slides, pulls them from the already-generated and reviewed post-test MCQ set (a different, harder set than Orientation's pre-test, corrected 2026-09-18) rather than writing fresh unreviewed questions inline. Reuses the existing live-session-deck engine and tokens. Routes to the existing closing-ceremony-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Closing Ceremony Generation · Post-Sales · SKILL.md (v1)

**What this produces.** One program Closing Ceremony deck, the same artifact `closing-ceremony-review/SKILL.md` already reviews, backward-looking wrap-up content, structurally distinct from Orientation.

**A Dry Run version is the same deck, not separate content.** Per the reviewer skill's own resolved note, an internal Dry Run rehearsal deck is this exact deck rehearsed internally, with any embedded assessment kept in for the rehearsal rather than linked externally. Don't generate two different decks, generate the one live/external version, Dry Run is an operational concern, not a separate generation target.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved, for what this program actually covered, the Key Takeaways section has to reflect the real curriculum, not a generic recap. **Added 2026-09-18, per `content-generation/SKILL.md` §2: whether this actually names specific real topics per day, not just section headers, is the single most load-bearing granular fact here.** If a day's rows are thin on real topic detail, don't stop, flag it plainly rather than let the Key Takeaways section quietly lean generic for that day.
- `Outputs/[Client]/deep-research.md`, for program-specific framing.

**Best-effort:**
- `Outputs/[Client]/mcq/post-test.docx`, if already generated and reviewed, as the source for any embedded assessment questions. **Corrected 2026-09-18: not the same file Orientation uses.** Real evidence from an actual e& delivery shows the pre-test and post-test are genuinely different questions at different difficulty levels, this is `mcq-generation`'s post-test output specifically, never the pre-test file.
- A similar past Closing deck as a structural reference.

---

## 2. Rules, reused from closing-ceremony-review, not reinvented

Generate against the confirmed recurring structure `closing-ceremony-review/SKILL.md` §1.1 checks for:

- A reflection prompt, what participants enjoyed or took away.
- A Key Takeaways recap specific to what this program actually covered, pulled from the Lesson Plan's real topics, never a generic template recap.
- A "Bridging Learning to Application" or concrete next-steps section.
- A feedback form link.
- A Post-Class/Program Test pointer, either linking out or embedding the assessment, per §3 below.
- A sign-off/closing slide.

**Embedded-assessment rule, generation-side.** If this deck embeds assessment questions as slides rather than linking externally, those questions come from `Outputs/[Client]/mcq/post-test.docx`, already generated and already reviewed by `mcq-reviewer`, don't write fresh questions directly into the deck. Writing new, unreviewed questions here would bypass the review this exact content type already has.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3: confirm the Key Takeaways section actually names real topics from the Lesson Plan, not boilerplate. If embedding assessment content, confirm it was pulled from the already-reviewed `mcq/post-test.docx`, not authored fresh inside this deck.

---

## 4. Where it gets saved

`Outputs/[Client]/closing-ceremony-deck/`, same per-engagement parent folder as everything else, format matching whatever the shared deck engine outputs.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:closing-ceremony-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `agent-loops` via the `skills:` frontmatter field. **No `generation-learnings/closing-ceremony.md` file exists yet**, deliberately, same reasoning as Orientation, per `generation-learnings/README.md`, this is a lower-iteration-volume artifact type, don't pre-build speculative infrastructure before a real repeat pattern shows up.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] All six structural sections from §2 present, none skipped
- [ ] Key Takeaways names real topics from the Lesson Plan, not a generic recap
- [ ] Embedded assessment content, if any, pulled from the already-reviewed `mcq/post-test.docx` (not the pre-test file), not authored fresh
- [ ] Generated the live/external deck, not a separate Dry Run artifact
- [ ] Built through the existing deck engine/tokens, not a new rendering mechanism
- [ ] Saved to `Outputs/[Client]/closing-ceremony-deck/`
- [ ] Handed to the existing `closing-ceremony-reviewer`, no bespoke review invented
