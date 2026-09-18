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
  -> Onboarding Form (collects fresh info from learners, incl. the lead-only section when applicable)
  -> Deep Research (builds on the Facts Sheet + onboarding form + transcripts + emails + precedent)
  -> Lesson Plan (first generated artifact, the day-by-day skeleton everything else builds against)
  -> Instructor Finalization (day-by-day roster, matched against the Lesson Plan)
  -> Demo (per day) and MCQ (pre-test / post-test / per-day in-session)
  -> Slide Content Planning (per day, reads that day's Demo + in-session MCQ, decides the real slide-by-slide content) -> Slides (Utkarsh's engine, renders the plan, doesn't decide content itself)
  -> Assignment / Project / Hands-On Guide / Orientation (reads pre-test) / Closing Ceremony (reads post-test) — independent of Content Planning, built straight from the Lesson Plan / Deep Research / their own MCQ file
  -> Review, per artifact (agent-loops mechanics) then the full-session content-review bundle (§6)
  -> Dry Run + Dry-Run Feedback (rehearsal, findings routed through the same fix loop) -> Live Delivery
```

**Corrected 2026-09-18: Demo and MCQ move before Slide Content Planning, not after.** Previously this diagram showed Content Planning feeding Slides/Demo/MCQ/etc as parallel siblings, which is backwards for two of them specifically. Content Planning is where the real story gets decided, and it can't build an accurate one without already knowing what the real demo does and what that day's quiz actually asks, guessing at either produces exactly the thin, generic content Content Planning was introduced to fix in the first place. This only applies to Demo and the per-day in-session MCQ, the artifacts that day's Content Plan actually reads from. Pre-test, post-test, Assignment, Project, and Hands-On Guide don't depend on Content Planning at all, they stay independent, built straight from the Lesson Plan and Deep Research.

A generation skill never invents its way around a missing upstream stage. If Deep Research hasn't run yet, say so and stop, don't generate a Lesson Plan against guessed context. Same discipline `discovery-fit-review` already applies to the Facts Sheet.

**Content creation is selective per engagement, not exhaustive.** Not every engagement needs every content type. If this one only needs slides and an MCQ set, build and loop just those two, don't generate demo, assignment, and project just because the skills exist. Per Utkarsh's own worked example: deep research gathers the detail once, then whichever content types this specific engagement actually needs get built, reviewed, and looped, nothing more.

---

## 1a. Where `Outputs/` actually lives

`Outputs/[Client]/` is always anchored to this plugin's own root, `post-sales/Outputs/[Client]/`, never relative to wherever an input file (a proposal, a reference doc) happens to live on disk. Those input files can sit anywhere, a shared drive folder, a client-specific directory, anything, but generated output always lands in one predictable place inside the plugin itself, the same place every other post-sales skill already reads from and writes to. A generation skill that infers the save location from an input file's own folder is not following this convention, even if it technically nests something under a folder named "Outputs."

**Corrected 2026-09-17, real recurring failure: a bare relative `Outputs/[Client]/...` path resolved wrong, repeatedly, on a real test day.** Every skill already states this convention correctly, the actual bug was execution-time: whatever session runs a generation command isn't reliably started with its working directory already at the plugin root, and a bare relative path silently resolves against whatever the current directory happens to be instead, once landing inside an unrelated client folder that coincidentally also had an `Outputs` subfolder sitting nearby. This has to work the same way regardless of whose machine the plugin is installed on or what absolute path it lives at, so the fix is never a hardcoded absolute path.

**Do this before every save (and every read of a prior-stage artifact):**
1. Locate the plugin's own root by finding the folder that contains `post-sales/.claude-plugin/plugin.json` (search from the current working directory, don't assume it's already there).
2. Build the full path as `<that folder>/post-sales/Outputs/[Client]/...`, never a bare `Outputs/[Client]/...` left for the shell to resolve on its own.
3. After saving, read the file back from that exact resolved path to confirm it actually landed there before declaring the step done, the same self-verification discipline §3 already requires for mechanical facts, applied here to the save location itself.

---

## 1b. Also saving to Google Drive, and where

**Added 2026-09-18, per Tanmaya/Utkarsh planning.** `Outputs/[Client]/` (§1a) is the plugin's own local copy, but it's local to whoever's machine ran the session. That's not enough for a pipeline more than one person touches, if one person generates the Lesson Plan and is out the next day, whoever picks up the next stage needs to reach it without depending on that first person's laptop. So everything also saves to a shared Drive location, live, the moment it's generated, not batched for later.

**Two separate Drive locations, never mixed, split by audience, not by file type:**

- **`B2B AI Programs`** (the existing Drive folder, `1tUMBGWfAKPzKA4hndaDjBe9hOHzMhza2`) — only artifacts a learner, instructor, or client stakeholder actually sees or receives: Session Deck, Demo, Orientation, Closing Ceremony, Assignment, Project, Hands-on/Setup Guide, Session Recap, Session Impact Report. **This is the exact folder Utkarsh's Curriculum Graph reads from** (`acceler-kg-sync`, `sync/build_curriculum_html.py`), so anything saved here becomes part of that graph once his sync job next runs.
- **`PostSalesPluginOutput`** (new, sibling folder, not inside `B2B AI Programs`) — everything that builds toward the above but isn't itself learner/client-facing: Discovery Facts Sheet, Onboarding Form, Deep Research, Lesson Plan, Content Plan, Instructor Roster, the MCQ master files, Dry-Run Feedback. **Deliberately kept out of `B2B AI Programs`** — the Curriculum Graph treats every subfolder inside it as a real delivered class module, so a working doc dropped in there would show up as if it were real class content. Keeping it in a separate folder that's simply never added to Utkarsh's known-programs list makes it automatically invisible to the graph, no filtering logic needed on either side.

**Folder shape inside `B2B AI Programs`**, matching the real existing convention, cohort now made explicit since the same client/program keeps recurring across cohorts:
```
B2B AI Programs/
  B2B [Client] [Program Name] - Cohort [N] ([Period])/
      Day 1/
        Live Class/
          Slides/
          Demo/
        Post Class/
          Recap/
          Impact Report/
      Day 2/  ... Day N/                    <- however many days THIS engagement's proposal actually states, never assume 4
      Orientation/
      Closing Ceremony/
      Assignment or Project/                (when applicable)
      Hands-on / VM Setup/                  (when applicable)
```

**Folder shape inside `PostSalesPluginOutput`**, mirroring the local `Outputs/[Client]/` structure so the two stay easy to reason about together:
```
PostSalesPluginOutput/
  [Client]/
    [Program Name] - Cohort [N] ([Period])/
      discovery-facts-sheet.md
      onboarding-form.docx
      deep-research.md
      lesson-plan.xlsx
      instructor-roster.md
      content-plan/
        day-1.md ... day-N.md
      mcq/
        pre-test.docx
        post-test.docx
        day-1-in-session.docx ... day-N-in-session.docx
      dry-run-feedback.md
```

**The known-programs list, a required step, not optional.** Utkarsh's Curriculum Graph only recognizes a `B2B AI Programs` subfolder as real delivered content if its exact name is in `B2B_CONTENT_PROGRAM_CLIENTS` (`acceler-kg-sync`, `sync/build_curriculum_html.py`, also mirrored in `sync/export_curriculum.py`). Creating a brand-new client/program folder there without also adding it to that list means the graph will silently never show it, no error, nothing visibly broken, it just never appears. So: before saving anything to a `B2B AI Programs` folder that doesn't already exist there, either add that exact folder name to the list yourself (prepared as part of the batched `acceler-kg-sync` change, not pushed separately) or clearly tell the user this step still needs doing. Never leave it silently undone.

**Testing.** Any test run uses `TEST - [whatever's being tested]` inside `B2B AI Programs`, and `TEST/` inside `PostSalesPluginOutput`. A `TEST -` folder must never be added to the real known-programs list, so it stays invisible to the live graph, same mechanism that keeps `PostSalesPluginOutput` invisible, just applied to test data specifically.

**How the write itself happens, for now.** No dedicated write-access service account exists yet, that's a later upgrade for unattended runs. Today, saving to Drive happens through whatever session is actually running the generation, using its own connected Drive access, the same way this plugin's development sessions already read real Drive content during planning.

---

## 2. Mandatory versus best-effort inputs

Not every input exists for every engagement. Each generation skill's own SKILL.md states which of its inputs are mandatory (generation stops and asks if missing) versus best-effort (used if present, the gap is stated plainly if not, never invented). This mirrors the MUST/SHOULD/NICE tiering `discovery-checklist` already uses, three tiers, not a binary required/optional.

**Corrected 2026-09-17, per Utkarsh's own instruction from the spec-kit discussion (2026-09-10): "no need to write that you should block deck generation, just clearly call out what is the input expected out of you."** That rule applies specifically to individual inputs within a stage that's otherwise runnable, an entirely different thing from an upstream pipeline stage not existing at all:
- **Upstream stage genuinely missing** (no Discovery Facts Sheet exists at all, no Lesson Plan exists at all) — this alone actually stops generation, there's nothing to build against. Per §1, never guess your way around this.
- **A specific mandatory input within an otherwise-available stage is missing or thin** (e.g. the Facts Sheet exists but doesn't state a success metric, or the user hasn't named a precedent) — don't stop. Name exactly what's expected (e.g. "these are the 5 inputs this needs, 1 was given, here's what's still missing"), proceed with what's available, and state plainly that providing the rest would improve the output. This is the spec-kit-style clarify-before-building spirit Utkarsh pointed at (github/spec-kit), applied without a hard block.

**Closed 2026-09-18** (parked 2026-09-17, done as checklist Phase A item 5): every generation skill now names its own single most load-bearing granular fact and flags it softly rather than blocking. `discovery-checklist` (the 3-month success metric) and `instructor-finalization` (Proposed vs Confirmed) already had this. Added to the remaining eleven: Deep Research (onboarding form's audience skill-level signal), Lesson Plan (real day count and per-day duration), MCQ (that day's Learning Objective column actually filled in), Assignment (starter-kit split, when one exists), Demo (Libraries/Tools column naming a specific tool), Project (shape signal, milestone/trap vs. documentation-brief), Hands-on Guide (account-scheme signal), Orientation and Closing Ceremony (concrete, engagement-specific outcomes/topics), Onboarding Form (strengthened, the outcome-tie question's real source), Slide Content Planning (Deep Research's precedent notes carrying real specifics). See each skill's own §1 for its exact wording.

For Deep Research specifically: mandatory inputs are the saved Discovery Facts Sheet (`Outputs/[Client]/discovery-facts-sheet.md`, from stage 1), the pre-sales proposal, and the learner onboarding form. Best-effort inputs are discovery call transcripts, the team-lead discovery form, client emails, and precedent from similar past engagements, same client or a similar one, B2B or B2C. See `deep-research/SKILL.md` for the full detail.

**A precedent engagement is a best-effort input for every generation skill, not only Deep Research.** Before generating from scratch, check `post-sales/knowledge/engagement-catalog.md`, a plain list of what real content already exists per past engagement. If whoever's running the generation hasn't already named a precedent client for this one (e.g. "follow LVT's project shape"), ask rather than assume no precedent applies. This is manual lookup for now, not an automatic matcher, see the catalog's own §Status.

**Added 2026-09-18, per Soham's own instruction on a real standup call: check that a fetched precedent actually is what it claims to be, before using it.** Once the Curriculum Graph connection lands (§1b), Deep Research becomes the main place this happens automatically, but `slide-content-planning`'s own precedent lookup carries the same real risk today. His exact worry: a system that ever serves the wrong thing once loses trust completely, even if everything else about it is right. So before treating a fetched precedent as real: confirm it actually names the client and day/program it claims to, a cheap surface check, not a deep audit, catching a wrong-node fetch before it quietly shapes generated content.

---

## 2a. Explicit human input always wins, at any step

**Added 2026-09-18, a general rule, not scoped to one skill.** Everything in §2 above is about handling input that's thin or missing. This is the opposite, deliberate, explicit case: whenever a human running a step states something directly, a slide count, a duration, a specific instructor, a specific tool, an exact figure, anything, that stated input overrides this skill's own default judgment for that run, full stop. Don't quietly apply a usual pattern, a typical density target, or a learned default over something a human just told you directly for this specific engagement.

This applies at every stage, not just generation, the same discipline a reviewer already applies to a human-approved fix per `agent-loops/SKILL.md`. A concrete example: if a human says "this day needs at least 50 slides, the hands-on work runs long," that stated number is what this run plans against, not whatever a skill's own usual pattern would have produced. The skill can still say so plainly if the explicit input seems to conflict with something else it knows (e.g. the stated day duration looks tight for that many slides), per §2's own soft-flag spirit, but it states the conflict and proceeds with what the human said, it doesn't silently substitute its own judgment instead.

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

**This is per-artifact, fast feedback right after one thing is generated, it is not the final gate.** Once every content type this engagement actually needs (per the selectivity note in §1) has been generated and individually reviewed, run `acceler-post-sales:content-review` once for the whole session bundle, it adds cross-artifact coherence and audience-fit checks no single-type reviewer can do alone, and produces the actual ship/don't-ship verdict. Don't treat the last individual artifact's Approve as the session being done, the bundle pass is still required before anything goes to a client or cohort.

---

## 6a. State the loop explicitly, don't just inherit it silently

*Utkarsh's own instruction, 2026-09-11: before finalizing a generation skill's summary, it has to say plainly that this loops with the reviewer's feedback over time.* Referencing `agent-loops` and `generation-learnings` in the `skills:` frontmatter is necessary but not sufficient on its own, each generation skill's own description writes this out in its own words: that its output gets reviewed, that findings feed `generation-learnings/<type>.md` once the promotion rule is met, and that this is expected to make the *next* generated artifact of that type better, not just fix the one instance in front of you. The architecture cannot end at create, review, get feedback, fix it manually, that's a one-off, not a loop, say so in the skill itself so it isn't left implicit.

---

## 7. Checklist
- [ ] Plugin root located via the `post-sales/.claude-plugin/plugin.json` anchor, save path built from that, never a bare relative `Outputs/[Client]/...` left for the shell to resolve
- [ ] Saved file read back from its resolved absolute path to confirm it actually landed there
- [ ] Also saved to the right Drive folder per §1b: learner/client-facing to `B2B AI Programs`, working docs to `PostSalesPluginOutput`, never mixed
- [ ] Day-count in any Drive path matches this engagement's actual proposal length, never hardcoded to 4
- [ ] If this created a brand-new `B2B AI Programs` client/program folder, it's been added to `B2B_CONTENT_PROGRAM_CLIENTS`, or the user's been clearly told this is still outstanding
- [ ] Test runs use `TEST -` / `TEST/` naming in both Drive locations, never added to the real known-programs list
- [ ] Every input tiered mandatory / best-effort / not applicable, not a binary required/optional
- [ ] Missing upstream stage (no Facts Sheet, no Deep Research) means stop and ask, never invent
- [ ] Only the content types this engagement actually needs get built, not every type by default
- [ ] Mechanically checkable facts (durations, counts, cross-references) self-verified before handoff
- [ ] `agent-loops` referenced in `skills:` frontmatter, its §2a / §2a-1 / §2b rules not restated
- [ ] Matching `generation-learnings/<type>.md` referenced in `skills:` frontmatter
- [ ] Output handed to the existing matching reviewer, no bespoke review process invented
- [ ] The skill's own description states the generation-review loop explicitly, per §6a, not left implicit
