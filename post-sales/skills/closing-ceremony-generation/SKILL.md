---
name: closing-ceremony-generation-skills-acceler-wrap-up-deck
description: "Generates one program Closing Ceremony deck by copying a real precedent deck (this client's own real prior one where it exists, otherwise the closest real precedent) and editing only the identified engagement-specific fields in place, native PPTX, not rebuilt through live-session-deck's HTML engine. Grounded 2026-09-24 in the real, actually-delivered e& AI Builder Program (Low Code) Closing deck, read directly, not assumed. Routes to the existing closing-ceremony-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Closing Ceremony Generation · Post-Sales · SKILL.md (v2)

**How this skill is started (added 2026-10-01):** every run begins with the start-up check in `content-generation/SKILL.md` §6c (name the mode per §6b, check what already exists for this client, ask at most 3 to 5 questions, write a light brief in adapt or standalone runs). Where this file says an input is mandatory or says to stop when one is missing, that describes a fresh build inside the full pipeline; in adapt or standalone runs the gap is handled the §6c way instead. Before building, also read `agent-loops/SKILL.md` (§2a, §2a-1, §2b) and this artifact type's `generation-learnings/` file where one exists, they are not loaded automatically.

**What this produces.** One program Closing Ceremony deck, the same artifact `closing-ceremony-review/SKILL.md` already reviews, backward-looking wrap-up content, structurally distinct from Orientation.

**Rewritten 2026-09-24, same real finding as Orientation.** Reading the real, actually-delivered e& Closing deck directly (`Closing notes | e& - AI Builder Accelerator (Low Code)`) showed the same pattern: a real Closing deck is a native PowerPoint/Slides file, mostly fixed structure, with a specific, identifiable handful of fields that change per engagement. Copy the real precedent, edit only the real swap zones, in place, per `content-generation/SKILL.md` §1e, not a fresh build through `live-session-deck`'s HTML engine.

**A Dry Run version is the same deck, not separate content.** Per the reviewer skill's own resolved note, an internal Dry Run rehearsal deck is this exact deck rehearsed internally, with any embedded assessment kept in for the rehearsal rather than linked externally. Don't generate two different decks, generate the one live/external version, Dry Run is an operational concern, not a separate generation target.

---

## 1. Inputs

**Mandatory:**
- **A real precedent Closing deck.** For this client, if one already exists, use it directly, otherwise the closest real precedent per `post-sales/knowledge/engagement-catalog.md`. Export it to `.pptx` as the working copy.
- `Outputs/[Client]/lesson-plan.xlsx`, approved, for what this program actually covered, the Key Takeaways section (§2, tier 2) has to reflect the real curriculum, not the precedent's own topics.
- `Outputs/[Client]/deep-research.md`, for program-specific framing.

**Best-effort:**
- `Outputs/[Client]/mcq/post-test.docx`, already generated and reviewed, as the source for the embedded assessment question slides (§2, tier 3), where this program embeds them rather than only linking out. **Not the same file Orientation uses.** Real evidence from the actual e& decks confirms pre-test and post-test are genuinely different questions at different difficulty levels, per `mcq-generation/SKILL.md` §2a, always `post-test.docx` here, never the pre-test file.

---

## 2. Three-tier structure, not a flat fixed/generate split

**Confirmed 2026-09-24 by reading the real e& deck slide by slide.** Same three-tier pattern as Orientation:

**Tier 1, fully fixed, reused byte-identical from the precedent:** the reflection prompt slide ("Which aspect of the program did you enjoy the most..."), the "Bridging Learning to Application" section (Start Small & Iterate / Creative Problem Solving / Navigate Organizational Realities / Build Your Implementation Portfolio), the general feedback ask, the closing "Plug In & Power Up" founder slide.

**Tier 2, same heading/structure, content retailored to this engagement:** Key Takeaways, real e& structure is three fixed headings (Conceptual Learning / Hands-on with Real World Tools / Evaluate like a Practitioner), same three headings every time, but the actual bullets under each one need to name what this specific cohort's sessions actually covered, pulled from the real Lesson Plan, never the precedent's own bullets carried over unchanged.

**Tier 3, fully swapped:** client/program name wherever it appears, the Post-Course Assessment instructions slide's real timing (which day, what time, per this engagement's actual schedule), the MS Form submission link, and the actual embedded assessment question slides (pulled verbatim from the already-reviewed `mcq/post-test.docx`, per §2a below, never authored fresh in the deck).

**Branding follows `content-generation/SKILL.md` §1d, independent of tier.** Acceler by default, even on a PowerUp-branded precedent, only kept as PowerUp when a human explicitly asks for it on this run.

---

## 2a. Embedded assessment, generation-side

If this deck embeds the post-course assessment as slides, those question slides come from `Outputs/[Client]/mcq/post-test.docx`, already generated and reviewed, copied in verbatim, never authored fresh inside this deck. Per `mcq-generation/SKILL.md` §2a, the real default shape is 10 single-correct MCQs + 2 subjective questions, no multi-select, the easier and more foundational of the two real assessments, not the harder one, unless a human specified different counts for this engagement's post-test.

---

## 2b. Two real defects found on Orientation's first live run, applied here pre-emptively

**Added 2026-09-24.** Closing uses the identical copy-a-real-precedent-and-edit-in-place method as Orientation (§1e), and Orientation's first live run surfaced two real defects in that method itself, confirmed by `orientation-reviewer` and a direct OOXML diff, not a hypothetical. Applying both here before Closing's own first real run, not waiting to rediscover the same thing twice:

- **Dedupe the precedent's own leftover content before saving.** A real precedent deck can carry its own edit history, two versions of the same slide left in from when the source file itself was updated between cohorts. Before saving, scan for near-duplicate slides and resolve which one is actually current, removing the stale one.
- **Edit text in place, don't clear-and-rebuild the shape.** Clearing a text box and rewriting it flattens the precedent's real formatting (bullets, spacing, per-run color) to uniform plain text. Edit the existing runs' text content directly, so the shape's own formatting survives untouched. Confirm by diffing the edited shape's XML against the precedent's own.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3:
- Confirm Tier 1 slides are untouched except branding.
- Confirm Key Takeaways' bullets actually name real topics from this engagement's Lesson Plan, not the precedent's own recap or generic boilerplate.
- Confirm every Tier 3 field is genuinely swapped, no leftover precedent client name, no wrong-day timing, no fabricated assessment question.
- If embedding assessment content, confirm it was pulled from the already-reviewed `mcq/post-test.docx` (never the pre-test file), not authored fresh.

---

## 4. Format and export

Per `content-generation/SKILL.md` §1e: native `.pptx`, edited in place from the real precedent file, source formatting/fonts/theme preserved untouched. **PDF is generated only after a human approves the PPTX**, not on every fix-loop round.

---

## 5. Where it gets saved

`Outputs/[Client]/closing-ceremony-deck/closing-ceremony-deck.pptx` (and `.pdf` once approved), same per-engagement parent folder as everything else, also pushed to `B2B AI Programs` per `content-generation/SKILL.md` §1b (learner-facing).

---

## 6. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:closing-ceremony-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. `agent-loops` is read at the start of the run, per the note at the top of this file. **No `generation-learnings/closing-ceremony.md` file exists yet**, deliberately, same reasoning as Orientation, per `generation-learnings/README.md`, this is a lower-iteration-volume artifact type, don't pre-build speculative infrastructure before a real repeat pattern shows up.

---

## 7. Checklist
- [ ] A real precedent deck identified and exported to `.pptx`, or the gap handled per `content-generation/SKILL.md` §6c, never generated from nothing
- [ ] Tier 1 slides (reflection prompt, Bridging Learning to Application, feedback ask, founder slide) copied untouched except branding
- [ ] Tier 2 (Key Takeaways) keeps the precedent's three headings but shows this engagement's real session content
- [ ] Tier 3 fields (client name, assessment timing, MS Form link, assessment questions) fully swapped, no leftover precedent content
- [ ] Embedded assessment content, if any, pulled from the already-reviewed `mcq/post-test.docx` (not the pre-test file), not authored fresh
- [ ] No near-duplicate leftover slides carried over from the precedent's own edit history, per §2b
- [ ] Edited text preserves the precedent's real formatting, edited in place not cleared-and-rebuilt, per §2b
- [ ] Branding matches Acceler default per §1d, unless a human explicitly asked for PowerUp
- [ ] Generated the live/external deck, not a separate Dry Run artifact
- [ ] Built by editing the real precedent PPTX in place, not regenerated through `live-session-deck`'s HTML engine
- [ ] PDF only generated after human approval, not every fix-loop round
- [ ] Saved to `Outputs/[Client]/closing-ceremony-deck/` locally and to `B2B AI Programs` per §1b
- [ ] Handed to the existing `closing-ceremony-reviewer`, no bespoke review invented
