---
name: agent-loops-skills-acceler-shared-loop-engineering-pattern
description: "The shared generate/fix -> evaluate -> re-verify loop pattern used by every Acceler review skill, and followed by every generation skill too (they read it at the start of each run). Deterministic success criteria, bounded iterations, progress/spin checks, maker/checker separation, human-gated apply, a baseline quality bar, prose-quality/anti-AI-tell checking verified against current published research, universal discovery-fidelity checking, an honest live-data boundary, full eval coverage (post-sales/evals/, 20 cases across all 16 agents), and the two-sided improvement loop (reviewer-side: eval set + Memories + future impact-report data; generation-side: post-sales/generation-learnings/, infrastructure ready, connects once generation agents exist). Grounded in Addy Osmani's practical loop-engineering framework. Referenced (via its own `skills:` frontmatter entry) by every reviewer agent in the family — edit here, not per-skill, when the shared mechanics themselves change."
metadata:
  type: reference
---

# Acceler Agent Loops — Shared Pattern — SKILL.md (v1)

**Why this exists.** Every review skill's fix loop was near-identical prose repeated in each of them. That's a maintenance hazard, a mechanics change would need the same edit in every review skill (15 today), and they'd drift. This file is the one place the mechanics live; each skill states only what's specific to its own artifact.

**Terminology, stated plainly.** Osmani's post distinguishes two primitives: *loops* run on a cadence (poll something repeatedly over time); *goals* run until a specific, measurable condition is met, then stop. Every fix loop in this plugin is a **goal**, not a cadence loop — it runs until a rule passes or the bound is hit. "Agent Loops" is the umbrella name for this project's loop-engineering work; the mechanism itself is goal-shaped. Don't build a recurring/scheduled loop where a bounded goal is what's actually needed.

---

## 1. The five rules every fix loop here follows

1. **Deterministic success criteria.** Every rule a hat checks resolves to PASS/FAIL from stated evidence — never "does this feel right." ("Every objective traces to a teaching artifact" is checkable; "the deck feels polished" is not.) This is why every review skill's §1 rubric is written as binary-flaggable rules.
2. **Explicit iteration bound.** Every fix loop stops at **2 rounds**. Not "keep trying until it's good."
3. **Per-turn progress check.** A retry only counts if the fix materially changed something. If round 2's fix is identical to round 1's, that's not a genuine second attempt — treat the bound as already exhausted, don't manufacture a 3rd try to compensate.
4. **Spin detection.** The same fix proposed twice with no change in verdict is a signal the finding isn't mechanically fixable — escalate rather than attempting a variation.
5. **Maker/checker separation, always — and as of this version, structurally enforced, not just instructed.** Every reviewer agent (`agents/*-reviewer.md`) ships with `disallowedTools: Write, Edit` (and `NotebookEdit` where relevant) — it has no Write or Edit tool, so it cannot change the artifact through the normal editing path (reviewers that need to run code keep Bash for execution only, and their instructions forbid using it to modify the artifact). Only `agents/content-fixer.md` has edit access, and it is only ever invoked with one pre-approved finding. This closes a real gap the instruction-only version had: a prompt telling an agent "don't edit files" can be ignored under pressure (confirmed happening once, in a research subagent, before this version); a tool restriction enforced at the agent-definition level cannot be.

---

## 2. The human gate (this plugin's addition on top of Osmani's pattern)

Osmani's post covers autonomous loops verifying against hard rules with no human in the middle. This plugin adds one more gate deliberately: **no fix is applied without explicit human approval**, because content quality includes judgment calls (tone, curriculum fit) hard rules can't fully capture — the human owns the merge, not the AI (the final tier and the human checklist are in `content-review/SKILL.md` §4 and §5). Full sequence:

```
Hat finds FAIL → propose {finding, evidence, suggested fix} → HUMAN APPROVES →
fresh fixer agent applies (blind to rationale, guardrail: artifact only) →
same original hat re-verifies fresh → PASS (done) or FAIL (round 2, same gate) →
still FAIL after 2 rounds → escalate to 🔴 Request Changes, never a 3rd attempt
```

**What to call a first-pass FAIL (added 2026-10-01).** A review skill's verdict table describes where things end up, so it has no row for "a FAIL was just found and no fix round has run yet." In the first full eval run several reviewers had to work this out for themselves. The answer, the same for every reviewer: report it as **not approvable, fix proposed, waiting on human approval**. It is not Approve and it is not yet Request Changes. It becomes Approve once the approved fix re-verifies as PASS, and Request Changes only if it hasn't converged after 2 rounds, or straight away where a skill says a finding escalates immediately (a fabricated quote, a leaked credential).

Between proposal and approval, the human isn't limited to a binary accept/reject — see each skill's reference to `explain` / `resolve` / `re-review` (`content-review/SKILL.md` §6).

**The human gate is two checkpoints, not one, confirmed by Acceler's own real module-creation checklist.** That checklist explicitly separates "reviewed internally" from "reviewed by an SME" as sequential, distinct gates — an internal reviewer catching format/process issues, then a subject-matter expert catching correctness/depth issues a generalist reviewer can't. Where a human approval step in any skill in this family is described as a single "human approves," read that as shorthand for "whoever the appropriate checkpoint is" — for content requiring SME sign-off (most hands-on/code-demo and project content), the fix loop's approval step should route to the SME, not just whichever human is closest, and both checkpoints' comments should be logged, not just the final one.

---

## 2a-2. The generation-to-review handoff is an action, not a mentioned step

**Confirmed happening on a real run (e& AI Builder Low Code Lesson Plan, 2026-09-16): the generating command's own instructions clearly said to hand off to its reviewer, and it didn't happen.** The run built the artifact, reported it, and stopped, treating "hand off to review" as a line it described rather than a further action it still had to take before ending its turn. This is the exact failure Ut independently flagged too: an instruction that lives only in a skill file competes with the much stronger pull of "the main deliverable is done, wrap up," and loses, even when it's written in plain language, even when it's the explicit last step in a numbered list.

**The fix is to state this as a hard completion condition, not a step to narrate.** Every generation command's own instructions must say, in these terms: *this command's turn is not finished until the matching reviewer has actually been invoked. Producing the artifact and reporting it without invoking the reviewer is an incomplete run, not a completed one, the same as skipping the self-verification step would be.* Applies to every generation command with a matching reviewer in this family, not just Lesson Plan, this is a shared-mechanics fix, made here once, not repeated per skill.

---

## 2a. Baseline quality bar — every reviewer applies this, in addition to its own rubric

*Grounded in Acceler's own real, recurring review feedback across multiple programs — these are documented, repeated corrections, not hypothetical nitpicks. Every `*-reviewer` agent in this family checks these regardless of content type, on top of its skill-specific §1 rules:*

- **Zero spelling/grammatical errors.** Confirmed recurring finding across real feedback ("There are minor grammatical errors at significant places," dozens of specific typos logged in real slide-by-slide review notes). Don't wait for a dedicated grammar pass — flag it as part of the normal rubric.
- **Formatting consistency within one artifact** — font size/style, heading capitalization (e.g. "seq2seq" capitalized inconsistently across slides in a real reviewed deck), notation (`/` vs `\` for alternatives — standard is `/`), page numbering present and consistently positioned.
- **No undefined acronym/term used before it's introduced** — a real recurring finding (I/P, O/P, POS, NER, TF, IDF used without definition in a reviewed deck). First use of any acronym or domain term needs an inline definition or expansion.
- **No duplicate content** (a real finding: an exact duplicate slide found in a reviewed deck).
- **Image/diagram citations present** where content is sourced externally, and low-resolution/pixelated images are flagged, not passed through.

This section is deliberately generic across artifact types — a skill's own §1 rubric is where content-specific rules live; this is the floor every artifact is held to regardless of type.

---

## 2a-1. Prose quality — generated content must read as human-written, technically strong, and instructionally sound

*User-stated requirement, embedded here so every reviewer inherits it rather than one skill owning it. Two layers: explicit style bans (below, verbatim) and AI-tell density (verified externally, not guessed — see the caveat before using it).*

**Explicit bans, prose only — never applied to code, quotes, or direct excerpts:**
- No em-dashes. Use a comma, parentheses, or two sentences instead.
- No 3-item rhetorical cadence (manufacturing "X, Y, and Z" for rhythm — a genuine enumerated list of five tools or ten checks is fine; the ban is on the *rhetorical* triad).
- No "it's not an X, it's a Y."
- No "X is not just a Y, it's the whole point."
- No antithesis-as-a-crutch ("not because it's easy, but because it's hard").

**AI-tell density — flag the pattern, not the word.** Verified against Wikipedia's actively-maintained "Signs of AI writing" page (cross-checked against independent word-frequency research; called "the best guide to spotting AI writing" by TechCrunch, Nov 2025) — and its own central finding is the one to hold onto: **no single word or phrase proves AI authorship. Only several of these co-occurring in the same passage is a real signal.** Never fail a passage for using "robust" once. Flag it when three or more of the following stack up in the same section:
- Overused vocabulary: *delve, leverage, foster, underscore, showcase, navigate, harness, pivotal, robust, intricate, nuanced, meticulous, crucial, boasts, elevate, unlock, streamline, tapestry, realm, testament, cornerstone, interplay.* This list drifts as models update — Wikipedia's own page notes "delve" already fell out of favor through 2025 — treat it as a living list, not a fixed one; a rubric edit that refreshes it needs a matching eval case (§8), not a silent swap.
- Copulative avoidance ("serves as," "stands as," "functions as" standing in for a plain "is").
- Negative parallelism as a crutch ("not only X but also Y") — same family as the antithesis ban above, extend the same instinct to it.
- Uniform paragraph rhythm and sentence length (low "burstiness") — real human technical writing varies.
- Vague hedged attribution ("industry reports show," "experts argue") with no one actually named.
- Rigid formulaic closings (a "Challenges and Future Outlook"-shaped section applied regardless of whether it fits the content).

**The specificity check — the actual counter-signal, not just a style rule.** Fluent-but-generic prose next to genuinely specific content is itself a tell (a real academic-detection heuristic, not house opinion): exact tool versions, real dataset names, actual sample sizes, field-specific caveats. Generated technical/instructional content that stays generic where a real subject-matter expert would name specifics is a FAIL on this ground alone, independent of the word list above.

**Citation and factual-claim integrity — elevated severity, not routine.** Any content making a factual claim with an attached citation, reference, or source (a project brief, a deep-research output, an MCQ explanation citing a paper or a number) needs that claim traced and verified as real, not assumed plausible. This is not hypothetical: Springer Nature retracted a 2025 machine-learning textbook after roughly two-thirds of its sampled citations turned out fabricated or substantially wrong. Treat an unverified or fabricated citation with the same severity as `impact-report-review`'s attribution-integrity rule (§1.1 there) — escalate to a human, don't let it ride through a routine fix-loop round.

---

## 2b. Discovery fidelity — every reviewer checks this, where a Facts Sheet exists

*User-stated requirement, made explicit as its own baseline section rather than left to one reviewer: "matching the details we have from the discovery checklist... are we making sure that it is filled? That should be a proper pipeline." This is not the same check as §2a (quality bar) or a per-skill curriculum-alignment rule — those check internal correctness; this checks the artifact against what the **client actually told us** during discovery.*

- Wherever `discovery-fit-review`'s Discovery Facts Sheet exists for this engagement, every reviewer in this family checks its artifact against the *relevant* facts on that sheet — not just `onboarding-form-review`, all of them. A deck built for a stated non-technical audience that reads as written for engineers, a hands-on-guide that assumes tool access the Facts Sheet says is IT-blocked, a project brief silent on a regulated-data constraint the client flagged — all of these are Discovery Fidelity failures, distinct from a curriculum-alignment failure, and should be labeled as such in the finding.
- This is a **completeness-of-fulfillment check, not a duplicate curriculum check**: the question isn't "does this artifact teach its stated objective" (that's §1 territory in each skill), it's "does this artifact honor what the client actually told us during discovery, and would the client recognize their own stated constraints reflected in it." An artifact can pass every curriculum-alignment rule and still fail this one.
- If no Discovery Facts Sheet exists for this engagement (discovery hasn't run, or predates this pipeline), say so plainly and skip this check rather than inventing facts to check against — same discipline as every other "don't invent objectives" rule in this family.
- This is the mechanism that makes discovery detail-capture an actual enforced pipeline, not a one-time form filled in and never checked again.

---

## 2c. Live data — what's actually wired, stated honestly

*Investigated directly (`acceler-kg-sync` repo, `knowledge/` folder) rather than assumed. Two different things are both called "the Knowledge Graph" and they are not equally live for this reviewer family's purposes — don't conflate them.*

- **Genuinely live:** `knowledge/files.json` (1,833 files as of last check), `graph.json`, `instr_candidates.json`, `INDEX.md`. `acceler-kg-sync` runs every 12h (GitHub Actions cron), pulls Drive + 4 instructor-rating Sheets into Supabase, and republishes these exact files into the repo-root `knowledge/` folder (the pre-sales plugin's folder, not `post-sales/knowledge/`) automatically — no plugin-side wiring needed, commands already just `Read` them. Each file record carries `client`, `program`, `doctype`, `tools`, `topics`, `duration`, `cohort`, and a `drive_url` back to the source. Any reviewer needing to locate a real artifact for an engagement (e.g. "find this client's actual Lesson Plan") can query this file directly (`grep`/`jq` over `knowledge/files.json` by `client`/`doctype`) instead of asking the human to paste a path — but note `duration`/`cohort` are frequently null in practice, don't assume they're populated.
- **Partly live, updated 2026-10-01:** the separate "Delivered Curriculum Graph" (`acceler-knowledge-graph.vercel.app/curriculum-graph`, scoped to actually-delivered B2B content) is now also exported as `post-sales/knowledge/curriculum-graph.json` by `acceler-kg-sync` (`sync/export_curriculum.py`), and `deep-research` and `slide-content-planning` read it. It holds programs, modules and files with their links. It does **not** hold per-session stated objectives, so the instruction across this reviewer family still stands: objectives come from the approved Lesson Plan or Facts Sheet, otherwise from the run's light brief (§4), and are never invented. Known gaps, parked for v2: program and module nodes often carry a generic folder link instead of the real one, and very large files (a 45MB deck) are silently skipped by the sync.
- **Still not built:** per-session objectives in that export. Adding them is a change to a different repo (`acceler-kg-sync`), not something this plugin can do on its own.

---

## 3. Guardrail (non-negotiable, every skill inherits this)

**The fixer may only edit the artifact under review. It must never edit the stated learning objectives, the rubric itself, or any source-of-truth input the artifact is checked against (Facts Sheet, onboarding data, chat or transcript evidence, schedule facts) to make a finding disappear.** This is the content-review equivalent of an agent "fixing" a failing test by rewriting the assertion instead of the code — a documented failure mode in agentic coding review (Osmani's agentic-code-review post). A finding that would only resolve by weakening the objective or the rule is, by definition, not mechanically fixable — it escalates per §1.4, it doesn't get "fixed" by moving the goalpost.

Note what this guardrail can and can't be: `content-fixer` technically *has* Edit access to any file it's pointed at, including a SKILL.md rubric — nothing in the plugin agent frontmatter schema can scope Edit down to "only this one artifact path" at definition time, since the path varies per invocation. This rule is therefore still instruction-enforced, not tool-enforced, unlike the maker/checker separation in §1.5. Treat any `content-fixer` output that touches a rubric or objectives file as an automatic Request-Changes escalation, not a fix to review normally.

---

## 4. Evaluator scope discipline

Keep every hat's rubric narrow and rule-based — "does this meet the stated rule," never a general "is this good" judgment. A goal evaluator "only examines if the hard rules have been met." Holistic or subjective judgment stays with the human checklist (`content-review/SKILL.md` §5), not with a hat — don't let a hat's rubric quietly expand into taste.

**Added 2026-10-01: review against the scope that was actually asked for.** A generation handoff now carries the mode, the scope, and (in adapt or standalone runs) a light brief, per `content-generation/SKILL.md` §6c. Every reviewer uses them:
- **Objectives source.** If a real Lesson Plan or Facts Sheet exists, check against that. If only a light brief exists, its objectives outline is the stated objectives for this review, and the reviewer says so in one line. Don't fall back to reconstructing objectives from a chat message.
- **Deliberately partial artifacts.** When the scope says only part of an artifact was asked for (a 7-slide opening deck, a single quiz, one demo of several), everything outside that scope is out of bounds for FAIL findings. A real review of a deliberately short Ferguson deck raised "missing Instructor, house rules, Quiz and Thank You slides" and "two objectives have no teaching slide" as HIGH failures, none of which were asked for. Note what's outside the scope once, as a single informational line ("out of scope for this run, needed before delivery: ..."), never as separate findings and never counted toward the verdict.
- **Inside the scope, nothing changes.** Every rule in the reviewer's own rubric applies in full to what was actually built.

---

## 5. Model routing (cost discipline — apply once volume justifies it)

Route mechanical fix-application to a faster/cheaper model tier; reserve the most capable model for the rubric judgment calls (the hats themselves) and anything human-facing. Not yet load-bearing at current review volume — noted for when this scales past occasional use.

---

## 6. How every review skill uses this

Each skill's own "Fix & re-verify loop" section states only what's artifact-specific: what "the artifact" means for that skill, and that skill's one-line guardrail restatement. The mechanics — round bound, progress/spin checks, maker/checker separation, the human gate — live here, once. Change loop mechanics here; don't hand-edit the same paragraph in every review skill.

*(There is no §7. A section was removed earlier and the numbering was kept, so existing pointers to §8 and §9 stay valid.)*

---

## 8. Eval coverage (regression protection, not just design confidence)

`post-sales/evals/` holds 20 golden cases, covering every agent in this family (some agents get more, where real-world risk is highest: `mcq-reviewer` and `deck-reviewer` have three each). Every case traces to a real document or a real, cited finding from this plugin's own grounding research — see `evals/README.md`. **A rubric rule with no case behind it is unverified prose, not a confirmed behavior** — treat "add/update a case" as part of making a rubric change, not an optional follow-up. `claude plugin eval` (the native harness) is gated to early access on this account as of this writing; these run as a manual protocol until that opens, written to migrate cleanly rather than inventing a parallel runner.

---

## 9. The two-sided improvement loop — reviewer over time, generation over time

*User-stated requirement: "agent loops which improves the generation over time, and reviewer over time." These are two different mechanisms with two different data sources — don't conflate them.*

### Reviewer improves over time (built, running)

1. **The eval set (§8).** Grows on every real false negative (a defect that shipped and shouldn't have) or false positive (a legitimate pattern wrongly flagged) — that's the trigger to add a case, not a calendar.
2. **Per-skill Memories logs.** A single dismissal is just one human's call on one instance. The **same finding dismissed repeatedly across different engagements** is a different signal — the rule itself is miscalibrated, not every instance a one-off. That's when a Memories pattern should become a rubric edit (with a matching eval case added, per §8), not stay a growing list of individually-dismissed items nobody revisits.
3. **Impact-report data** (PRD Phase 3, not yet flowing — this repo has no mechanism today to read `impact-report-review`/`session-recap-review` output back into a rubric). The intended future input for recalibrating the Pacing & Difficulty / Engagement Ratio lenses (already seeded as difficulty-label and slide-count-vs-duration checks in `mcq-review`/`deck-review`) against real delivery outcomes instead of static rules.

### Generation improves over time (connected, early)

**Updated 2026-10-01: the generation skills now exist and this side is connected.** Each generation skill reads its content type's file at the start of a run, and `generation-learnings/mcq.md` already carries promoted entries from real fix-loop history (2026-09-24). The other files are still at the candidate stage. The target that connection plugs into is `post-sales/generation-learnings/`, one file per content type, structurally parallel to each reviewer's Memories log but pointed the other direction: Memories stops a *reviewer* re-flagging something a human dismissed; a Generation Learning stops a *generator* making the same mistake before a reviewer has to catch it at all.

**The record a fix-loop resolution needs to carry**, so it can feed this later: `{content_type, rule, evidence, fix_applied, engagement, date}`. Every `content-fixer` invocation already produces this shape implicitly (the finding it was given + what it changed) — nothing new needs building to start capturing it, as of 2026-10-06 it is logged: every run appends its findings, dismissals and the person's own manual changes to the client's `run-notes.md`, per `content-generation/SKILL.md` §6d. That file is what maintainers read to apply the promotion rule below, until a shared memory replaces it in v2.

**The promotion rule** (`generation-learnings/README.md`): the same rule failing **≥3 times** across different generated artifacts of the same content type promotes from "routine fix-loop occurrence" to a Generation Learning entry — a directive the generation agent should follow proactively. A few rules are seeded as *candidates* ahead of that threshold where the legacy-content evidence gathered during this session's grounding pass was already overwhelming (see `generation-learnings/mcq.md`, `project.md`, `lesson-plan.md`) — flagged as candidates, not promoted, until real generation fix-loop history exists yet to actually confirm the ≥3 threshold.

**The connection contract, for whoever wires this up:** a generation skill reads its content type's `generation-learnings/<type>.md` file at the start of every run, as its own SKILL.md tells it to. Only agents have a `skills` frontmatter field that preloads files automatically, skills and commands don't, so for generation this is an explicit read, not a preload.

---

## 10. Checklist
- [ ] Every rubric rule is stated as a checkable PASS/FAIL condition, not a vibe
- [ ] Fix loops obey the 2-round bound, with progress and spin checks applied before burning a round
- [ ] No fix applied without explicit human approval
- [ ] Fixer-guardrail (artifact-only editing) is intact in every skill referencing this pattern
- [ ] Maker and checker are always different agent calls
- [ ] §2a Baseline Quality Bar applied by every reviewer, not just skill-specific rules
- [ ] §2a-1 Prose Quality applied: explicit bans checked absolutely, AI-tell density checked as co-occurrence (3+ signals), never a single word in isolation
- [ ] Any cited factual claim traced and verified, not assumed plausible — escalated immediately if fabricated or unverifiable, per §2a-1
- [ ] §2b Discovery Fidelity checked wherever a Facts Sheet exists, explicitly skipped (not invented) where it doesn't
- [ ] §2c's live-vs-not-yet-live data boundary respected — query `knowledge/files.json` for real, don't invent a curriculum.json query that doesn't exist yet
- [ ] Any rubric change gets a corresponding case added/updated in `evals/` (§8) — a rule with no golden case is unverified prose
- [ ] Fix-loop resolutions are captured in the `{content_type, rule, evidence, fix_applied, engagement, date}` shape (§9) so generation-learnings promotion can actually happen once generation is connected
