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
- The learner onboarding form responses. **Added 2026-09-18, per `content-generation/SKILL.md` §2: the single most load-bearing granular fact here is a real audience skill-level signal**, not just that responses exist. If the form came back with skill-matrix ratings unanswered or every response reading identically vague, that's thin, not missing, don't stop, but name it plainly in the output ("audience skill level is thin, ratings weren't meaningfully filled in") rather than quietly picking a pacing that might be wrong.

**Best-effort, use if present, state plainly if not, never invent:**
- Discovery call transcripts.
- The team-lead discovery form, where the engagement has one.
- Client emails and other communications, ask the user where these live if they want them included, don't assume they're findable on their own.
- Precedent: similar past work with this client, or a similar client, B2B or B2C. Check `post-sales/knowledge/engagement-catalog.md` first, it lists what real content already exists per past engagement (audience, tools, which content types, where the files sit). If whoever's running this hasn't already named a specific precedent client, ask which past engagement this one is closest to rather than assuming none exists.
- **Added 2026-09-18: `post-sales/knowledge/curriculum-graph.json`, once it exists.** This is Utkarsh's real Curriculum Graph, published automatically by his sync job (per `content-generation/SKILL.md` §1b), not a live lookup, just a file to read like any other. If it's there: look for a client node matching this engagement (or the closest real match, same client or a similar one), walk to its program and module nodes, and use their real file names/Drive links as grounded precedent, the same idea as the catalog above but automatic and more precise, since it's built from actually-delivered content, not a manually kept list. **Per `content-generation/SKILL.md` §2's precedent-verification rule, confirm whatever node you land on actually names the client/program it claims to before using it**, don't trust a fetched match blindly. **If the file doesn't exist yet** (real as of 2026-09-18, his sync job hasn't published it yet), that's fine, fall back to the catalog and asking a human, exactly as already described above, don't block waiting for it. **Before reading it, make sure it's actually the latest copy**: `git fetch origin` then `git merge origin/master` (never a plain `git pull`) on whatever branch this session is on. The bot that publishes this file commits straight to `master` on its own schedule, unrelated to when this session happens to start, so whatever's already on disk could be stale, a quick fetch+merge first is what makes it current.
- **Added 2026-09-24, real gap found on a live test run: check how many real demos each day actually has, not just what they're about.** Real evidence (e& Low-Code) showed Days 1 and 2 each pair a small warm-up demo (a basic skill or concept exercise) with one main hands-on build, a two-part shape, not one demo per day. This is easy to miss if you only read the module/file names off the graph, walk into that day's real Drive folder (the module node's own file listing, or its Demo Files subfolder if the graph points at one) and count what's actually there: how many distinct real demo files/docs exist, and whether one is clearly a shorter warm-up before a bigger build. Note this explicitly per day in §2's output, don't just name the one main demo and stop there.

---

## 2. What it produces

A short, structured brief, not a data dump, shaped around what actually gets built next:

- **Audience and pacing calibration**, who's in the room, their level, how fast to go, from the Facts Sheet and onboarding answers.
- **Day-by-day themes**, organized by day where the sources support it, not just a flat list of everything discussed. **Added 2026-09-24: for each day, state how many real distinct demos precedent shows (not just the one main build), and flag if there's a real warm-up-then-main-build pattern**, per the real Days 1-2 finding in §1 above.
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
- [ ] Each day's real demo count checked (not assumed to be one), warm-up-then-main pattern flagged where precedent shows it, per §1/§2
- [ ] `agent-loops` §2a / §2a-1 inherited even though there's no dedicated reviewer
- [ ] Saved to `Outputs/[Client]/deep-research.md`
- [ ] Human approval obtained before Lesson Plan (or anything else) reads it
