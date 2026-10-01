---
name: content-generation-skills-acceler-shared-pipeline-pattern
description: "The shared pattern every Acceler content-generation skill follows: which inputs are mandatory vs best-effort, self-verification before handoff, and how generation connects to agent-loops (fix-loop mechanics, baseline quality bar) and generation-learnings (per-type accumulated directives). Read at the start of the run by every generation skill (Deep Research, Lesson Plan, Slides, MCQ, Assignment, Project) the same way every reviewer references agent-loops."
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

A generation skill never invents its way around a missing upstream stage. If Deep Research hasn't run yet in a fresh pipeline build, say so and stop (or go standalone with a light brief, the human chooses, per §6c), don't generate a Lesson Plan against guessed context. Same discipline `discovery-fit-review` already applies to the Facts Sheet.

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

**Status, decided 2026-10-01: in v1 the plugin never uploads to Drive on its own, it uploads when a human asks.** Everything below still decides *where* each file belongs, and that part is in force today. What changed is *when the upload happens*: at the end of a run, list every file produced and the exact Drive folder it belongs in (learner or client-facing files to `B2B AI Programs`, working files to `PostSalesPluginOutput`), then leave it there. A human either uploads those files by hand, or tells the plugin to upload them ("push the Day 1 deck and demo to Drive"), and in that case the plugin does it through the session's own connected Drive access, to the folders this section names. Unattended, automatic upload of every artifact the moment it's generated, through a dedicated service account, is v2. Wherever this plugin's skills say a file is "also pushed" or "also saved" to a Drive folder per this section, read that as "belongs in that folder, uploaded when a human asks or does it by hand in v1."

**Creating a Google Form is a separate case and works today.** `onboarding-form-generation` and `mcq-generation` can build a real Google Form when a human asks for one after approving the content. That is an explicit, human-requested action, the same on-request principle as above, not an automatic save.

**Added 2026-09-18, per Tanmaya/Utkarsh planning.** `Outputs/[Client]/` (§1a) is the plugin's own local copy, but it's local to whoever's machine ran the session. That's not enough for a pipeline more than one person touches, if one person generates the Lesson Plan and is out the next day, whoever picks up the next stage needs to reach it without depending on that first person's laptop. So everything also belongs in a shared Drive location (the original intent was a live save the moment it's generated, that automatic part is now v2, see the status note above).

**Two separate Drive locations, never mixed, split by audience, not by file type:**

- **`B2B AI Programs`** (the existing Drive folder, `1tUMBGWfAKPzKA4hndaDjBe9hOHzMhza2`) — only artifacts a learner, instructor, or client stakeholder actually sees or receives: Session Deck, Demo, Orientation, Closing Ceremony, Assignment, Project, Hands-on/Setup Guide, Session Recap, Session Impact Report. **This is the exact folder Utkarsh's Curriculum Graph reads from** (`acceler-kg-sync`, `sync/build_curriculum_html.py`), so anything saved here becomes part of that graph once his sync job next runs.
- **`PostSalesPluginOutput`** (real, created 2026-09-18: `1XTJNdwNfv5irwS4ezjRbTDU6NONwfw4Y`, sibling folder, not inside `B2B AI Programs`) — everything that builds toward the above but isn't itself learner/client-facing: Discovery Facts Sheet, Onboarding Form, Deep Research, Lesson Plan, Content Plan, Instructor Roster, the MCQ master files, Dry-Run Feedback. **Deliberately kept out of `B2B AI Programs`** — the Curriculum Graph treats every subfolder inside it as a real delivered class module, so a working doc dropped in there would show up as if it were real class content. Keeping it in a separate folder that's simply never added to Utkarsh's known-programs list makes it automatically invisible to the graph, no filtering logic needed on either side. Its `TEST/` subfolder (`1FVs3f6ZW588qiGOrIcklS-DpmEAjIwfx`) already exists too.

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
      slide-content-plan-day-1.md ... slide-content-plan-day-N.md   (same file names as the local Outputs folder)
      mcq/
        pre-test.docx
        post-test.docx
        day-1-in-session.docx ... day-N-in-session.docx
      dry-run-feedback.md
      light-brief.md                         (only in adapt or standalone runs, per §6c)
```

**The known-programs list, a required step, not optional.** Utkarsh's Curriculum Graph only recognizes a `B2B AI Programs` subfolder as real delivered content if its exact name is in `B2B_CONTENT_PROGRAM_CLIENTS` (`acceler-kg-sync`, `sync/build_curriculum_html.py`, also mirrored in `sync/export_curriculum.py`). Creating a brand-new client/program folder there without also adding it to that list means the graph will silently never show it, no error, nothing visibly broken, it just never appears. So: before saving anything to a `B2B AI Programs` folder that doesn't already exist there, either add that exact folder name to the list yourself (prepared as part of the batched `acceler-kg-sync` change, not pushed separately) or clearly tell the user this step still needs doing. Never leave it silently undone.

**Testing.** Real, created 2026-09-18: `TEST - PostSales` inside `B2B AI Programs` (`1Fb_iGLtV6bZZuI2M7enKlfja4qQv5fSd`), and `TEST/` inside `PostSalesPluginOutput` (`1FVs3f6ZW588qiGOrIcklS-DpmEAjIwfx`). A `TEST -` folder must never be added to the real known-programs list, so it stays invisible to the live graph, same mechanism that keeps `PostSalesPluginOutput` invisible, just applied to test data specifically.

**How the write itself happens.** In v1, only on request, per the status note at the top of this section: the run ends with a list of files and their Drive folders, and a human either uploads them or tells the plugin to, which it then does through the session's connected Drive access. No dedicated write-access service account exists yet, that is the v2 upgrade that makes this automatic and unattended. Reading from Drive during a run (a real reference deck, a proposal) is unaffected and works today through the session's own connected Drive access.

---

## 1c. HTML is the format for learner-facing artifacts; PPTX/PDF are on-request exports, not separate builds

**Added 2026-09-18.** Any learner-facing artifact this pipeline builds as HTML (the session deck, and Demo per `demo-generation/SKILL.md`, going forward) follows the same export rule, one default, one explicit-ask exception, reusing pre-sales's own already-working pattern rather than inventing a new one:

- **Default: screenshot-per-slide/section, assembled into a PPTX**, per the pre-sales plugin's `pptx-deck/SKILL.md` (repo-root `skills/pptx-deck/`) and its already-documented method (headless-render each section at 2×, assemble with `python-pptx`). Fast, pixel-perfect, never drifts from the HTML. **Corrected 2026-09-24, unified with §1e's rule: PDF is generated only once a human asks for it or approves the export**, not automatically alongside every PPTX just because the rendering step makes it cheap, this saves real time and tokens the same way it does for Orientation/Closing.
- **Only on an explicit human ask to type-edit the PPTX afterward**: fall back to the native, editable rebuild instead (the native-rebuild notes (`PPTX_Deck_Skills.md`, a workspace file that is not kept in this repo, a human has to supply it)'s pattern, real shapes/text boxes, not images), per §2a, an explicit request always overrides the default.
- The HTML file itself stays the source of truth either way. Keep its content in one reusable data block so whichever export path runs doesn't drift from it.

Don't build a third, new conversion mechanism for Demo or any future HTML artifact, point it at this same rule.

---

## 1d. Branding: Acceler by default, PowerUp on explicit request

**Added 2026-09-24.** The company was previously branded PowerUp before becoming Acceler. Any artifact that carries a company logo or name (session deck cover, Orientation, Closing Ceremony) defaults to current Acceler branding, real logo file at `post-sales/knowledge/brand-assets/acceler-logo-dark.svg`. Some ongoing client relationships that started under the old brand may still expect PowerUp branding, real logo file at `post-sales/knowledge/brand-assets/powerup-logo.png`, kept permanently for exactly this. If a human explicitly asks for PowerUp branding for a specific run, use it, don't silently default to Acceler and don't silently swap a PowerUp request back to Acceler either, this is the same explicit-input-always-wins rule as §2a below, applied to branding specifically.

**Asked once per engagement, not once per artifact.** Per `discovery-fit-review/SKILL.md` §1.2, this is a field on the Discovery Facts Sheet, set at the very start of the pipeline, not re-asked at every generation stage. A generation skill reads it from there; it only actively asks a human when the Facts Sheet's Branding field is genuinely unset and there's a real reason to check (e.g. a precedent search turns up this client's own past content already PowerUp-branded).

---

## 1e. PPTX-native artifacts (Orientation, Closing Ceremony): reuse the real deck directly, don't rebuild through the HTML engine

**Added 2026-09-24.** §1c above is for artifacts this pipeline builds as HTML first (session deck, Demo). Orientation and Closing Ceremony are different: the real, already-delivered versions of both are native PowerPoint/Slides decks, mostly fixed company template content with a small number of real engagement-specific fields, not something to generate fresh through `live-session-deck`'s token/component system. See `orientation-generation/SKILL.md` and `closing-ceremony-generation/SKILL.md` for the full detail, in short:

- Start from a real precedent deck (this client's own real prior one where it exists, otherwise the closest real precedent per `post-sales/knowledge/engagement-catalog.md`), exported to `.pptx`.
- Edit only the identified engagement-specific fields in place (python-pptx, same tooling already used for PPTX export elsewhere in this pipeline), preserving the source file's real formatting, fonts, and layout untouched. No new design tokens, no HTML rebuild.
- Output is `.pptx`, native and fully editable.
- **PDF is generated only after a human approves the PPTX**, not on every fix-loop pass, this saves real time and tokens across what can be several review rounds before approval.

---

## 2. Mandatory versus best-effort inputs

Not every input exists for every engagement. Each generation skill's own SKILL.md states which of its inputs are mandatory (a fresh pipeline build stops and asks when the whole upstream stage is missing, see the two cases below and §6c) versus best-effort (used if present, the gap is stated plainly if not, never invented). This mirrors the MUST/SHOULD/NICE tiering `discovery-checklist` already uses, three tiers, not a binary required/optional.

**Corrected 2026-09-17, per Utkarsh's own instruction from the spec-kit discussion (2026-09-10): "no need to write that you should block deck generation, just clearly call out what is the input expected out of you."** That rule applies specifically to individual inputs within a stage that's otherwise runnable, an entirely different thing from an upstream pipeline stage not existing at all:
- **Upstream stage genuinely missing** (no Discovery Facts Sheet exists at all, no Lesson Plan exists at all) — this alone actually stops generation, there's nothing to build against. Per §1, never guess your way around this. **Narrowed 2026-10-01:** this hard stop applies to a fresh build running as a step of the full pipeline. In adapt mode, or when a human asks for one artifact on its own, §6c's light brief is what gets built against instead, the command doesn't stop.
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

## 3a. Don't trust a prior round's fix once an upstream input changes

**Added 2026-09-24, real bug found on a live test run.** A day's Lesson Plan was fixed once against one version of Deep Research. Deep Research was later revised with new findings (a real demo-count check that didn't exist yet in the version the fix was checked against). The next run claimed that day was "already reconciled" without actually re-checking it against the new version, it just remembered the day had been touched before and assumed that was still enough. It wasn't, the new finding was never reflected.

**The rule, general, not scoped to Lesson Plan or to demo counts specifically:** if something this skill reads (Deep Research, the Lesson Plan, a prior stage's output, anything upstream) gets a newer version after this skill's own output already exists, don't assume the existing output is still correct just because it was checked once before. Re-check it against the *current* version of that input before claiming anything is fine, reconciled, or already handled. "Already fixed" is a claim about the input version it was fixed against, it doesn't carry forward automatically once that input changes again. This applies anywhere in the pipeline the same shape can repeat, e.g. Orientation built from one Lesson Plan version, then the Lesson Plan changes again.

---

## 4. Inheriting the baseline quality bar and prose rules

Every generation skill inherits `agent-loops/SKILL.md` §2a (baseline quality bar), §2a-1 (prose quality, explicit style bans plus AI-tell density), and §2b (Discovery Fidelity, checked against the Facts Sheet) directly. Reference `agent-loops` in the `skills:` frontmatter field the same way every reviewer does. Don't restate these rules per generation skill, that's exactly the five-times-repeated-prose problem `agent-loops` itself was written to avoid.

---

## 5. Connecting to generation-learnings

Reference the matching file in `generation-learnings/<type>.md` by reading it at the start of the run (skills have no `skills:` frontmatter field, only agents do), the same connection contract `generation-learnings/README.md` already defines. One pre-seeded entry applies regardless of content type from day one: the Prose Quality section (§2a-1 above), per that README's own note, its evidence is already stronger than the usual promotion bar it sets for everything else.

---

## 6. Handoff to review

Once a generation skill produces its artifact, it hands off to the matching reviewer (`lesson-plan-review`, `deck-review`, `mcq-review`, and so on) using `agent-loops`'s fix-loop mechanics unchanged. Generation doesn't define its own review process, it uses the one that already exists.

**This is per-artifact, fast feedback right after one thing is generated, it is not the final gate.** Once every content type this engagement actually needs (per the selectivity note in §1) has been generated and individually reviewed, run `acceler-post-sales:content-review` once for the whole session bundle, it adds cross-artifact coherence and audience-fit checks no single-type reviewer can do alone, and produces the actual ship/don't-ship verdict. Don't treat the last individual artifact's Approve as the session being done, the bundle pass is still required before anything goes to a client or cohort.

---

## 6a. State the loop explicitly, don't just inherit it silently

*Utkarsh's own instruction, 2026-09-11: before finalizing a generation skill's summary, it has to say plainly that this loops with the reviewer's feedback over time.* Referencing `agent-loops` and `generation-learnings` at the start of a run is necessary but not sufficient on its own, each generation skill's own description writes this out in its own words: that its output gets reviewed, that findings feed `generation-learnings/<type>.md` once the promotion rule is met, and that this is expected to make the *next* generated artifact of that type better, not just fix the one instance in front of you. The architecture cannot end at create, review, get feedback, fix it manually, that's a one-off, not a loop, say so in the skill itself so it isn't left implicit.

---

## 6b. Three ways any generation skill gets invoked, not just one

**Added 2026-09-27.** Everything above describes the default: building an artifact fresh from this same engagement's own upstream pipeline (Deep Research, Lesson Plan, etc.), generated earlier in the same run. That's real, but it's only one of three real situations a human can actually be in when they ask for something. Every generation skill needs to work in all three, independently, not just as a step inside the full sequential flow, someone can walk up and ask for a single artifact on its own, with nothing else run first.

**Mode 1, fresh build.** The default already described in this file: build from this engagement's own real upstream artifacts. Full review, per §6, same as always.

**Mode 2, adapt from a real reference.** The human says, in effect, "this one's basically like that other one, with these differences," or hands over a real file on the spot, one that wasn't produced by this engagement's own pipeline at all, a past engagement's real artifact, or something entirely external. This is the same real approach already built for `orientation-generation` and `closing-ceremony-generation` (copy the real reference, swap only what's actually different), generalized here so any artifact type can use it, not just those two. The reference can come from `engagement-catalog.md`, or be handed over directly at runtime, either is fine, same as any other best-effort precedent input per §2. **This still gets the full review in §6, the same bar as Mode 1, no shortcut.** Real evidence for why: the two real defects found in Orientation's first live run (a leftover duplicate slide, a formatting-collapse bug) weren't in the original reference, they got introduced during the adapting step itself. Being grounded in something real doesn't mean the adaptation was done correctly, only a full check confirms that.

**Mode 3, tweak something that already exists for this exact engagement.** The human already has a real version of this artifact, generated by this pipeline earlier, or handed over as-is, and wants specific, named changes made to it, not a rebuild. Apply only the described changes, preserve everything else untouched, the same discipline `content-fixer` already applies to an approved reviewer finding. **This gets a light, targeted check scoped to just what changed**, not a full re-review of the whole artifact, that would be real waste with no added safety. But it's not zero-review either, confirm the specific change actually landed, and that nothing else moved, the same self-verification discipline §3 already requires elsewhere.

**A standalone request is not a fourth mode.** "Just build me the demo file" with nothing upstream and no reference is still Mode 1, a fresh build, only without the earlier stages behind it. §6c's light brief is what stands in for them.

**How to tell which mode a request is in:** look at what the human actually hands over or references at the start. Nothing existing yet, pointing only at upstream pipeline artifacts, Mode 1. A reference from somewhere else, a past engagement or an external file, Mode 2. An existing version of this exact artifact for this exact engagement, plus a specific list of changes, Mode 3.

**Whatever gets handed over at runtime, in any mode, gets saved to its proper `Outputs/[Client]/...` path per §1a**, not just used once and discarded, so the next stage (or the next person picking up this engagement) can read it too, not just whoever happened to be in this specific conversation.

---

## 6c. The start-up check, run first by every generation command

**Added 2026-10-01, per Tanmaya.** Most real requests are not a clean run of the full pipeline. The common case is "here's a real deck (or quiz, or demo), change it for this other client, here are a few details," sometimes with a lesson plan attached, sometimes with only raw content, sometimes just "build me the demo file" with nothing else. Every generation command, from Lesson Plan and MCQs through to Recap, has to handle all of these on its own. This is the one place that says how, each command points here rather than restating it.

**Run these four steps before building anything:**

1. **Name the mode** per §6b, in one line the human can see ("Mode: Adapt. The e& Day 1 deck is the starting point, rebuilt for Ferguson."). Decide it from what was handed over, never from whether the human mentioned review.
2. **Look at what already exists for this client** under `Outputs/[Client]/` (path per §1a): Discovery Facts Sheet, Deep Research, Lesson Plan, slide content plan, and any earlier version of this same artifact. A real upstream artifact always wins over anything derived below, never overwrite one with a derived draft.
3. **Handle whatever is missing, by mode:**
   - **Fresh build, as a step in the full pipeline:** §2's rule holds, a genuinely missing upstream stage stops this command. Say which stage is missing and offer the human two choices: run that stage first (name its command), or go ahead standalone with a light brief (below). Don't quietly write an unreviewed stand-in for the missing stage and carry on. A real run did exactly that on 2026-09-29, a Lesson Plan command wrote its own `deep-research.md` inline instead of asking.
   - **Adapt from a reference, or a standalone build of one artifact:** don't stop. Write a **light brief** (below) from the reference plus whatever the human gave, show it, then build.
   - **Tweak:** nothing is needed and nothing is asked, go straight to the change.
4. **Ask only what can't be worked out.** The command the human started is the one that asks, there is no separate intake step. At most 3 to 5 questions, only for facts that can't be read from the handed-over material or from `Outputs/[Client]/` (typically: audience and level, tools, day and duration, instructor). Everything else gets a stated assumption, flagged in the brief, not a question. Answers are saved into the brief so no later command asks the same thing again, the same asked-once principle as §1d.

**The light brief.** One short file, `Outputs/[Client]/light-brief.md`, holding: client and program, audience and level, tools, day and duration, branding, instructor if known, which reference was used and what differs from it, the **scope** the human asked for (for example "7 slides only: cover, agenda, timing, prompt engineering section, demo hand-off"), a short **objectives outline** (3 to 6 objectives covering exactly that scope), and a list of assumptions made. Its first line is `Status: light draft, derived from a reference and the human's brief, not from a scored discovery or an approved lesson plan.` It exists so the build has stated facts to work from and the reviewer has objectives to check against, a real adapt run without one left the reviewer checking a deck against a one-line chat message. It is not a replacement for Deep Research or a Lesson Plan: when the real stage runs later, the real artifact supersedes it, and the brief gets a line saying so.

**Scope travels with the handoff.** When handing off to the reviewer (§6), pass the mode, the scope, and the light brief's path. A deliberately partial artifact is reviewed against its stated scope, see `agent-loops/SKILL.md` §4.

---

## 7. Checklist
- [ ] Plugin root located via the `post-sales/.claude-plugin/plugin.json` anchor, save path built from that, never a bare relative `Outputs/[Client]/...` left for the shell to resolve
- [ ] Saved file read back from its resolved absolute path to confirm it actually landed there
- [ ] Run ended with the list of produced files and the Drive folder each belongs in per §1b (learner/client-facing to `B2B AI Programs`, working docs to `PostSalesPluginOutput`, never mixed), with nothing uploaded unless a human asked for it in v1
- [ ] Day-count in any Drive path matches this engagement's actual proposal length, never hardcoded to 4
- [ ] If this created a brand-new `B2B AI Programs` client/program folder, it's been added to `B2B_CONTENT_PROGRAM_CLIENTS`, or the user's been clearly told this is still outstanding
- [ ] Test runs use `TEST -` / `TEST/` naming in both Drive locations, never added to the real known-programs list
- [ ] Branding defaults to Acceler unless a human explicitly asked for PowerUp, per §1d
- [ ] Orientation/Closing built by editing a real precedent PPTX in place, per §1e, not regenerated through the HTML deck engine; PDF only generated after human approval
- [ ] Every input tiered mandatory / best-effort / not applicable, not a binary required/optional
- [ ] Start-up check run first, per §6c: mode named in one line, existing `Outputs/[Client]/` artifacts checked, at most 3 to 5 questions asked
- [ ] Missing upstream stage in a fresh pipeline build means stop and ask (run the stage, or go standalone with a light brief), never a silent inline stand-in; in adapt or standalone mode a light brief is written and shown instead, per §6c
- [ ] Mode, scope and the light brief's path passed to the reviewer at handoff
- [ ] Only the content types this engagement actually needs get built, not every type by default
- [ ] Mechanically checkable facts (durations, counts, cross-references) self-verified before handoff
- [ ] If an upstream input has a newer version than what this output was last checked against, re-verified against the current version, not assumed still fine from a prior round, per §3a
- [ ] `agent-loops` read at the start of the run (skills and commands have no `skills:` frontmatter field, only agents do), its §2a / §2a-1 / §2b rules not restated
- [ ] Matching `generation-learnings/<type>.md` read at the start of the run, where one exists
- [ ] Output handed to the existing matching reviewer, no bespoke review process invented
- [ ] The skill's own description states the generation-review loop explicitly, per §6a, not left implicit
- [ ] Invocation mode identified (fresh build / adapt from a reference / tweak an existing artifact) from what the human actually handed over, per §6b, not assumed to be Mode 1 by default
- [ ] Mode 2 (adapt from a reference) got the full review, same bar as Mode 1, no shortcut just because it's grounded in something real
- [ ] Mode 3 (tweak) got a scoped, targeted check on exactly what changed, not skipped and not a full re-review
- [ ] Anything handed over at runtime saved to its real `Outputs/[Client]/...` path, not used once and discarded
