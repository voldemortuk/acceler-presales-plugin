---
name: content-generation-skills-acceler-shared-pipeline-pattern
description: "The shared pattern every Acceler content-generation skill follows: which inputs are mandatory vs best-effort, self-verification before handoff, and how generation connects to agent-loops (fix-loop mechanics, baseline quality bar) and generation-learnings (per-type accumulated directives). Referenced (via `skills:` frontmatter) by every generation skill (Deep Research, Lesson Plan, Slides, MCQ, Assignment, Project) the same way every reviewer references agent-loops."
metadata:
  type: reference
---

# Acceler Content Generation · Shared Pattern · SKILL.md (v1)

**Why this exists.** `agent-loops/SKILL.md` already defines the fix-loop mechanics and baseline quality bar every reviewer, and every future generator, inherits. What it doesn't cover is the generation step itself: how a generator gathers what it needs, decides what's required versus optional, checks its own mechanical facts before handing off, and connects to `generation-learnings/`. This file is that missing half, written once, referenced by every content-generation skill instead of restated per type.

---

## 1. Where generation sits in the pipeline

```
Deal closes
  -> Discovery Checklist (re-run inside post-sales, produces the Facts Sheet)
  -> Deep Research (builds on the Facts Sheet + onboarding form + transcripts + emails + precedent)
  -> Lesson Plan (first generated artifact, the day-by-day skeleton everything else builds against)
  -> Slides / Demo / MCQ / Assignment / Project (generated from the Lesson Plan + Deep Research)
  -> Review (agent-loops mechanics, unchanged)
```

A generation skill never invents its way around a missing upstream stage. If Deep Research hasn't run yet, say so and stop, don't generate a Lesson Plan against guessed context. Same discipline `discovery-fit-review` already applies to the Facts Sheet.

---

## 2. Mandatory versus best-effort inputs

Not every input exists for every engagement. Each generation skill's own SKILL.md states which of its inputs are mandatory (generation stops and asks if missing) versus best-effort (used if present, the gap is stated plainly if not, never invented). This mirrors the MUST/SHOULD/NICE tiering `discovery-checklist` already uses, three tiers, not a binary required/optional.

For Deep Research specifically: mandatory inputs are the pre-sales proposal and the learner onboarding form. Best-effort inputs are discovery call transcripts, the team-lead discovery form, client emails, and precedent from similar past engagements, same client or a similar one, B2B or B2C.

---

## 3. Self-verification before handoff, fill the gap review can't

A generator checks anything mechanically checkable itself, before submitting for review, rather than waiting for a reviewer to catch it. This isn't optional polish, it's an already-confirmed real gap: `generation-learnings/lesson-plan.md`'s seeded candidate found duration math not summing to the stated day length in every sampled Lesson Plan, none of them had self-checked it. Any generation skill with an equivalent mechanical fact, durations, counts, cross-references between its own sections, computes and verifies it at creation time, not after.

---

## 4. Inheriting the baseline quality bar and prose rules

Every generation skill inherits `agent-loops/SKILL.md` §2a (baseline quality bar), §2a-1 (prose quality, explicit style bans plus AI-tell density), and §2b (Discovery Fidelity, checked against the Facts Sheet) directly. Reference `agent-loops` in the `skills:` frontmatter field the same way every reviewer does. Don't restate these rules per generation skill, that's exactly the five-times-repeated-prose problem `agent-loops` itself was written to avoid.

---

## 5. Connecting to generation-learnings

Reference the matching file in `generation-learnings/<type>.md` via the `skills:` frontmatter field, the same connection contract `generation-learnings/README.md` already defines. One pre-seeded entry applies regardless of content type from day one: the Prose Quality section (§2a-1 above), per that README's own note, its evidence is already stronger than the usual promotion bar it sets for everything else.

---

## 6. Handoff to review

Once a generation skill produces its artifact, it hands off to the matching reviewer (`lesson-plan-review`, `deck-review`, `mcq-review`, and so on) using `agent-loops`'s fix-loop mechanics unchanged. Generation doesn't define its own review process, it uses the one that already exists.

---

## 7. Checklist
- [ ] Every input tiered mandatory / best-effort / not applicable, not a binary required/optional
- [ ] Missing upstream stage (no Facts Sheet, no Deep Research) means stop and ask, never invent
- [ ] Mechanically checkable facts (durations, counts, cross-references) self-verified before handoff
- [ ] `agent-loops` referenced in `skills:` frontmatter, its §2a / §2a-1 / §2b rules not restated
- [ ] Matching `generation-learnings/<type>.md` referenced in `skills:` frontmatter
- [ ] Output handed to the existing matching reviewer, no bespoke review process invented
