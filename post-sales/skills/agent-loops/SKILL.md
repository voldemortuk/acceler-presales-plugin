---
name: agent-loops-skills-acceler-shared-loop-engineering-pattern
description: "The shared generate/fix -> evaluate -> re-verify loop pattern used by every Acceler review skill, and the reference future generation agents (Lesson Plan, Slide, MCQ, Assignment, Project) should follow too. Deterministic success criteria, bounded iterations, progress/spin checks, maker/checker separation, human-gated apply, a baseline quality bar, universal discovery-fidelity checking, an honest live-data boundary, full eval coverage (post-sales/evals/, 18 cases across all 16 agents), and the two-sided improvement loop (reviewer-side: eval set + Memories + future impact-report data; generation-side: post-sales/generation-learnings/, infrastructure ready, connects once generation agents exist). Grounded in Addy Osmani's practical loop-engineering framework. Referenced by every review skill in the family — edit here, not per-skill, when the shared mechanics themselves change."
metadata:
  type: reference
---

# Acceler Agent Loops — Shared Pattern — SKILL.md (v1)

**Why this exists.** Every review skill's fix loop was near-identical prose repeated five times. That's a maintenance hazard — a mechanics change would need five synchronized edits, and they'd drift. This file is the one place the mechanics live; each skill states only what's specific to its own artifact.

**Terminology, stated plainly.** Osmani's post distinguishes two primitives: *loops* run on a cadence (poll something repeatedly over time); *goals* run until a specific, measurable condition is met, then stop. Every fix loop in this plugin is a **goal**, not a cadence loop — it runs until a rule passes or the bound is hit. "Agent Loops" is the umbrella name for this project's loop-engineering work; the mechanism itself is goal-shaped. Don't build a recurring/scheduled loop where a bounded goal is what's actually needed.

---

## 1. The five rules every fix loop here follows

1. **Deterministic success criteria.** Every rule a hat checks resolves to PASS/FAIL from stated evidence — never "does this feel right." ("Every objective traces to a teaching artifact" is checkable; "the deck feels polished" is not.) This is why every per-type rubric (`deck-review` §1, `code-demo-review` §1, `mcq-review` §1, `assignment-review` §1) is written as binary-flaggable rules.
2. **Explicit iteration bound.** Every fix loop stops at **2 rounds**. Not "keep trying until it's good."
3. **Per-turn progress check.** A retry only counts if the fix materially changed something. If round 2's fix is identical to round 1's, that's not a genuine second attempt — treat the bound as already exhausted, don't manufacture a 3rd try to compensate.
4. **Spin detection.** The same fix proposed twice with no change in verdict is a signal the finding isn't mechanically fixable — escalate rather than attempting a variation.
5. **Maker/checker separation, always — and as of this version, structurally enforced, not just instructed.** Every reviewer agent (`agents/*-reviewer.md`) ships with `disallowedTools: Write, Edit` (and `NotebookEdit` where relevant) — it cannot apply a fix even if it tried to. Only `agents/content-fixer.md` has edit access, and it is only ever invoked with one pre-approved finding. This closes a real gap the instruction-only version had: a prompt telling an agent "don't edit files" can be ignored under pressure (confirmed happening once, in a research subagent, before this version); a tool restriction enforced at the agent-definition level cannot be.

---

## 2. The human gate (this plugin's addition on top of Osmani's pattern)

Osmani's post covers autonomous loops verifying against hard rules with no human in the middle. This plugin adds one more gate deliberately: **no fix is applied without explicit human approval**, because content quality includes judgment calls (tone, curriculum fit) hard rules can't fully capture — "the human owns the merge, not the AI" (`content-review/SKILL.md` §4). Full sequence:

```
Hat finds FAIL → propose {finding, evidence, suggested fix} → HUMAN APPROVES →
fresh fixer agent applies (blind to rationale, guardrail: artifact only) →
same original hat re-verifies fresh → PASS (done) or FAIL (round 2, same gate) →
still FAIL after 2 rounds → escalate to 🔴 Request Changes, never a 3rd attempt
```

Between proposal and approval, the human isn't limited to a binary accept/reject — see each skill's reference to `explain` / `resolve` / `re-review` (`content-review/SKILL.md` §6).

**The human gate is two checkpoints, not one, confirmed by Acceler's own real module-creation checklist.** That checklist explicitly separates "reviewed internally" from "reviewed by an SME" as sequential, distinct gates — an internal reviewer catching format/process issues, then a subject-matter expert catching correctness/depth issues a generalist reviewer can't. Where a human approval step in any skill in this family is described as a single "human approves," read that as shorthand for "whoever the appropriate checkpoint is" — for content requiring SME sign-off (most hands-on/code-demo and project content), the fix loop's approval step should route to the SME, not just whichever human is closest, and both checkpoints' comments should be logged, not just the final one.

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

## 2b. Discovery fidelity — every reviewer checks this, where a Facts Sheet exists

*User-stated requirement, made explicit as its own baseline section rather than left to one reviewer: "matching the details we have from the discovery checklist... are we making sure that it is filled? That should be a proper pipeline." This is not the same check as §2a (quality bar) or a per-skill curriculum-alignment rule — those check internal correctness; this checks the artifact against what the **client actually told us** during discovery.*

- Wherever `discovery-fit-review`'s Discovery Facts Sheet exists for this engagement, every reviewer in this family checks its artifact against the *relevant* facts on that sheet — not just `onboarding-form-review`, all of them. A deck built for a stated non-technical audience that reads as written for engineers, a hands-on-guide that assumes tool access the Facts Sheet says is IT-blocked, a project brief silent on a regulated-data constraint the client flagged — all of these are Discovery Fidelity failures, distinct from a curriculum-alignment failure, and should be labeled as such in the finding.
- This is a **completeness-of-fulfillment check, not a duplicate curriculum check**: the question isn't "does this artifact teach its stated objective" (that's §1 territory in each skill), it's "does this artifact honor what the client actually told us during discovery, and would the client recognize their own stated constraints reflected in it." An artifact can pass every curriculum-alignment rule and still fail this one.
- If no Discovery Facts Sheet exists for this engagement (discovery hasn't run, or predates this pipeline), say so plainly and skip this check rather than inventing facts to check against — same discipline as every other "don't invent objectives" rule in this family.
- This is the mechanism that makes discovery detail-capture an actual enforced pipeline, not a one-time form filled in and never checked again.

---

## 2c. Live data — what's actually wired, stated honestly

*Investigated directly (`acceler-kg-sync` repo, `knowledge/` folder) rather than assumed. Two different things are both called "the Knowledge Graph" and they are not equally live for this reviewer family's purposes — don't conflate them.*

- **Genuinely live:** `knowledge/files.json` (1,833 files as of last check), `graph.json`, `instr_candidates.json`, `INDEX.md`. `acceler-kg-sync` runs every 12h (GitHub Actions cron), pulls Drive + 4 instructor-rating Sheets into Supabase, and republishes these exact files into this plugin's `knowledge/` folder automatically — no plugin-side wiring needed, commands already just `Read` them. Each file record carries `client`, `program`, `doctype`, `tools`, `topics`, `duration`, `cohort`, and a `drive_url` back to the source. Any reviewer needing to locate a real artifact for an engagement (e.g. "find this client's actual Lesson Plan") can query this file directly (`grep`/`jq` over `knowledge/files.json` by `client`/`doctype`) instead of asking the human to paste a path — but note `duration`/`cohort` are frequently null in practice, don't assume they're populated.
- **Not yet live for structured queries:** the separate "Delivered Curriculum Graph" (`acceler-knowledge-graph.vercel.app/curriculum-graph`, scoped to actually-delivered B2B content) only exports as `index.html` — browsable, not machine-readable. There is no `curriculum.json` a reviewer agent could query for per-session stated objectives. This means the existing instruction across every skill in this family — "objectives sourced from the Curriculum KG / Day-by-Day doc, ask if missing, never invent" — is still the *correct* behavior, not a placeholder waiting to be replaced: there's genuinely no live objectives feed to switch to yet.
- **Concrete next step, not yet built:** extend `acceler-kg-sync/sync/export.py` (or add a sibling `export_curriculum.py`) to emit a `curriculum.json` alongside the existing HTML build, in the same shape `files.json` already uses — day/session-level records with stated objectives, not just document metadata. That's what would let `discovery-fit-review`, `lesson-plan-review`, and `deck-review` query real objectives instead of requiring a human to supply the Day-by-Day doc every time. This is a change to a different repo (`acceler-kg-sync`, Python + Supabase + GitHub Actions), not something this plugin's skills can do alone — flagging it as the actual dependency, not quietly working around it.

---

## 3. Guardrail (non-negotiable, every skill inherits this)

**The fixer may only edit the artifact under review. It must never edit the stated learning objectives or the rubric itself to make a finding disappear.** This is the content-review equivalent of an agent "fixing" a failing test by rewriting the assertion instead of the code — a documented failure mode in agentic coding review (Osmani's agentic-code-review post). A finding that would only resolve by weakening the objective or the rule is, by definition, not mechanically fixable — it escalates per §1.4, it doesn't get "fixed" by moving the goalpost.

Note what this guardrail can and can't be: `content-fixer` technically *has* Edit access to any file it's pointed at, including a SKILL.md rubric — nothing in the plugin agent frontmatter schema can scope Edit down to "only this one artifact path" at definition time, since the path varies per invocation. This rule is therefore still instruction-enforced, not tool-enforced, unlike the maker/checker separation in §1.5. Treat any `content-fixer` output that touches a rubric or objectives file as an automatic Request-Changes escalation, not a fix to review normally.

---

## 4. Evaluator scope discipline

Keep every hat's rubric narrow and rule-based — "does this meet the stated rule," never a general "is this good" judgment. A goal evaluator "only examines if the hard rules have been met." Holistic or subjective judgment stays with the human checklist (`content-review/SKILL.md` §5), not with a hat — don't let a hat's rubric quietly expand into taste.

---

## 5. Model routing (cost discipline — apply once volume justifies it)

Route mechanical fix-application to a faster/cheaper model tier; reserve the most capable model for the rubric judgment calls (the hats themselves) and anything human-facing. Not yet load-bearing at current review volume — noted for when this scales past occasional use.

---

## 6. How the five review skills use this

Each skill's own "Fix & re-verify loop" section states only what's artifact-specific: what "the artifact" means for that skill, and that skill's one-line guardrail restatement. The mechanics — round bound, progress/spin checks, maker/checker separation, the human gate — live here, once. Change loop mechanics here; don't hand-edit the same paragraph in five places.

---

## 8. Eval coverage (regression protection, not just design confidence)

`post-sales/evals/` holds 18 golden cases, one per agent in this family (some agents get more, where real-world risk is highest — `mcq-reviewer`, `impact-report-reviewer`). Every case traces to a real document or a real, cited finding from this plugin's own grounding research — see `evals/README.md`. **A rubric rule with no case behind it is unverified prose, not a confirmed behavior** — treat "add/update a case" as part of making a rubric change, not an optional follow-up. `claude plugin eval` (the native harness) is gated to early access on this account as of this writing; these run as a manual protocol until that opens, written to migrate cleanly rather than inventing a parallel runner.

---

## 9. The two-sided improvement loop — reviewer over time, generation over time

*User-stated requirement: "agent loops which improves the generation over time, and reviewer over time." These are two different mechanisms with two different data sources — don't conflate them.*

### Reviewer improves over time (built, running)

1. **The eval set (§8).** Grows on every real false negative (a defect that shipped and shouldn't have) or false positive (a legitimate pattern wrongly flagged) — that's the trigger to add a case, not a calendar.
2. **Per-skill Memories logs.** A single dismissal is just one human's call on one instance. The **same finding dismissed repeatedly across different engagements** is a different signal — the rule itself is miscalibrated, not every instance a one-off. That's when a Memories pattern should become a rubric edit (with a matching eval case added, per §8), not stay a growing list of individually-dismissed items nobody revisits.
3. **Impact-report data** (PRD Phase 3, not yet flowing — this repo has no mechanism today to read `impact-report-review`/`session-recap-review` output back into a rubric). The intended future input for recalibrating the Pacing & Difficulty / Engagement Ratio lenses (already seeded as difficulty-label and slide-count-vs-duration checks in `mcq-review`/`deck-review`) against real delivery outcomes instead of static rules.

### Generation improves over time (infrastructure ready, not yet connected)

**Not built by guessing at generation agents that don't exist yet.** Per the user's own stated plan: "Once Tanmaya has created the generation I will connect it." What's built now is the *target* that connection plugs into — `post-sales/generation-learnings/`, one file per content type, structurally parallel to each reviewer's Memories log but pointed the other direction: Memories stops a *reviewer* re-flagging something a human dismissed; a Generation Learning stops a *generator* making the same mistake before a reviewer has to catch it at all.

**The record a fix-loop resolution needs to carry**, so it can feed this later: `{content_type, rule, evidence, fix_applied, engagement, date}`. Every `content-fixer` invocation already produces this shape implicitly (the finding it was given + what it changed) — nothing new needs building to start capturing it, it just isn't being logged anywhere durable yet.

**The promotion rule** (`generation-learnings/README.md`): the same rule failing **≥3 times** across different generated artifacts of the same content type promotes from "routine fix-loop occurrence" to a Generation Learning entry — a directive the generation agent should follow proactively. A few rules are seeded as *candidates* ahead of that threshold where the legacy-content evidence gathered during this session's grounding pass was already overwhelming (see `generation-learnings/mcq.md`, `project.md`, `lesson-plan.md`) — flagged as candidates, not promoted, since no live generation fix-loop history exists yet to actually confirm the ≥3 threshold.

**The connection contract, for whoever wires this up:** a generation skill references its content type's `generation-learnings/<type>.md` file via the `skills` frontmatter field, same pattern every reviewer already uses to reference `agent-loops` — so learnings preload automatically rather than needing to be pasted into a prompt by hand.

---

## 10. Checklist
- [ ] Every rubric rule is stated as a checkable PASS/FAIL condition, not a vibe
- [ ] Fix loops obey the 2-round bound, with progress and spin checks applied before burning a round
- [ ] No fix applied without explicit human approval
- [ ] Fixer-guardrail (artifact-only editing) is intact in every skill referencing this pattern
- [ ] Maker and checker are always different agent calls
- [ ] §2a Baseline Quality Bar applied by every reviewer, not just skill-specific rules
- [ ] §2b Discovery Fidelity checked wherever a Facts Sheet exists, explicitly skipped (not invented) where it doesn't
- [ ] §2c's live-vs-not-yet-live data boundary respected — query `knowledge/files.json` for real, don't invent a curriculum.json query that doesn't exist yet
- [ ] Any rubric change gets a corresponding case added/updated in `evals/` (§8) — a rule with no golden case is unverified prose
- [ ] Fix-loop resolutions are captured in the `{content_type, rule, evidence, fix_applied, engagement, date}` shape (§9) so generation-learnings promotion can actually happen once generation is connected
