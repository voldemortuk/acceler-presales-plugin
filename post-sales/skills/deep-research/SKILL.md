---
name: deep-research-skills-acceler-post-sales-client-synthesis
description: "Synthesizes everything already known about a client, the Discovery Facts Sheet, the proposal, onboarding answers, transcripts, emails, similar past work, into one research brief that Lesson Plan (and optionally other stages) builds against. Not web research: internal context synthesis only. Lives inside acceler-post-sales, not a separate plugin. No dedicated reviewer, human approval only."
metadata:
  type: reference
---

# Acceler Deep Research · Post-Sales · SKILL.md (v1)

**What this is, and what it deliberately isn't.** Deep research here does not mean going on the internet and researching a topic. It means pulling together everything Acceler already knows about this specific client, conversations, proposals, forms, past work, into one place, so Lesson Plan (and anything else that needs it) doesn't start from a blank page. Internal synthesis, not external search.

---

## 1. Inputs

Follows `content-generation/SKILL.md` §2's tiering, mandatory versus best-effort, not a binary required/optional.

**Mandatory, won't run without these:**
- `Outputs/[Client]/discovery-facts-sheet.md`, produced by stage 1. Always exists by the time this runs, that stage gates the pipeline.
- The detailed pre-sales proposal.
- The learner onboarding form responses.

**Best-effort, use if present, state plainly if not, never invent:**
- Discovery call transcripts.
- The team-lead discovery form, where the engagement has one.
- Client emails and other communications, ask the user where these live if they want them included, don't assume they're findable on their own.
- Precedent: similar past work with this client, or a similar client, B2B or B2C. Check `post-sales/knowledge/engagement-catalog.md` first, it lists what real content already exists per past engagement (audience, tools, which content types, where the files sit). If whoever's running this hasn't already named a specific precedent client, ask which past engagement this one is closest to rather than assuming none exists.

---

## 2. What it produces

A short, structured brief, not a data dump, shaped around what actually gets built next:

- **Audience and pacing calibration**, who's in the room, their level, how fast to go, from the Facts Sheet and onboarding answers.
- **Day-by-day themes**, organized by day where the sources support it, not just a flat list of everything discussed.
- **Concrete use cases and examples**, pulled from this client's actual business, transcripts, or emails, never generic placeholders.
- **Tools and constraints**, carried forward from the Facts Sheet, not re-derived.
- **Precedent notes**, what worked in similar past engagements, if any exist, sourced from `post-sales/knowledge/engagement-catalog.md` plus whatever precedent the human running this named explicitly.
- **Gaps**, stated honestly wherever a best-effort source wasn't available, not filled in with something plausible-sounding.

---

## 3. No dedicated reviewer, and why that's intentional

Unlike the content types with a matching `*-reviewer` agent, deep research doesn't get one. Its job is to supplement Lesson Plan and whatever else reads it, it isn't itself a client-facing artifact with a correctness bar the way a deck or MCQ set has. A human (Tanmaya or the instructor) reads and approves it before anything downstream starts, that's the gate, not a formal reviewer pass. This may be revisited once volume grows, not needed at current scale.

It still inherits `agent-loops/SKILL.md` §2a (baseline quality bar) and §2a-1 (prose quality) directly though, the human reading it still shouldn't be reading AI-tell-heavy prose or sloppy formatting, that bar doesn't go away just because there's no dedicated reviewer agent.

---

## 4. Where it gets saved

`Outputs/[Client]/deep-research.md`, same per-engagement folder the Facts Sheet already lives in.

---

## 5. Who consumes it, kept flexible

Lesson Plan is always the first consumer, but this isn't hardwired to only feed Lesson Plan. Per the user call: some engagements have little existing precedent, so deep research matters more and can inform instructor finalization or content creation directly too; others already have enough internal context that Lesson Plan alone is sufficient. Don't force this into every stage automatically, let whoever's running the pipeline decide per engagement.

---

## 6. Handoff

Once approved by a human, Lesson Plan reads `Outputs/[Client]/deep-research.md` as a required input, per `content-generation/SKILL.md` §6.

---

## 7. Checklist
- [ ] Mandatory inputs present (Facts Sheet, proposal, onboarding form), or the run stops and asks rather than guessing
- [ ] Best-effort sources used where available, gaps stated plainly where not, nothing invented
- [ ] Output shaped as the five sections in §2, not a raw dump of source material
- [ ] `agent-loops` §2a / §2a-1 inherited even though there's no dedicated reviewer
- [ ] Saved to `Outputs/[Client]/deep-research.md`
- [ ] Human approval obtained before Lesson Plan (or anything else) reads it
