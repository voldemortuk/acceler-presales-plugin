---
name: content-review-skills-acceler-session-bundle-orchestrator
description: "How to run the Acceler session-level content review — orchestrates deck-review, code-demo-review, mcq-review, assignment-review, hands-on-guide-review, project-review, then adds the one check none of them can do alone: cross-artifact coherence across the whole session bundle. Produces the final PR-vocabulary verdict (Approve/Comment/Request Changes) and the human sign-off checklist. Companion to (and depends on) deck-review, code-demo-review, mcq-review, assignment-review, hands-on-guide-review, project-review."
metadata:
  type: reference
---

# Acceler Content Review — Session Bundle (Orchestrator) — SKILL.md (v2)

**What changed from v1:** the original single-skill design conflated "review each artifact" with "check they agree with each other." Splitting into per-type skills (`deck-review`, `code-demo-review`, `mcq-review`, `assignment-review`, `hands-on-guide-review`) exposed that most of what looked like a "holistic" hat was actually within-one-artifact checks (worked-example ordering, scaffolding fade) that belong in `code-demo-review`. What's left here is genuinely irreducible to a single-artifact check.

**5th and 6th artifact types added:** `hands-on-guide-review` covers the standalone tool-access/setup doc (e.g. `VM & AI Tools SetUp`, `Pre-Requisites Setup`) — distinct from the deck's own in-deck setup slides and from the code-demo notebook. `project-review` covers capstone/multi-milestone projects — confirmed structurally distinct from a simple assignment (milestone-staged, planted-bug grading, a verified reference solution as the real answer key). Both feed this skill's coherence check: a setup guide listing a tool the code-demo never uses, or a capstone project that doesn't build on anything the session actually taught, are exactly the kind of cross-artifact drift this skill exists to catch.

**Confirmed by a 5-thread survey of ~15 real Acceler program folders (Bosch, Nucleus, Cornerstone, Deloitte, ETS, e& variants ×6, Lowe's, LVT, Yettel, PPF Hungary) that every rubric in this skill family is now grounded in real, cited examples, not assumptions.** One open item from that survey, deliberately not resolved: Cornerstone's "Session 2 SME Copy" doc mixes lesson-narrative, live-demo, and an assignment-with-rubric in one file — doesn't cleanly fit any of the 6 skills. One instance isn't enough to justify a 7th skill; watch for more before deciding whether it's a genuine new type or gets split across existing ones like the MCQ/subjective hybrid was.

---

## 1. Rules — Cross-Artifact Coherence only (this skill owns coherence; audience fit lives in its own skill)

*This is the only rubric this skill file owns — specifically, `coherence-reviewer`'s rubric. Format correctness, technical execution, item-writing validity, and single-artifact objective alignment are already owned by the per-type skills — don't re-check them here. `audience-fit-reviewer`'s rubric (calibration against the real cohort and program expectations — a different question from internal consistency) lives entirely in `audience-fit-review/SKILL.md`; this orchestrator runs it per §2 but doesn't restate its rules here, for the same don't-duplicate-and-drift reason.*

- Every MCQ/assignment item maps back to something actually taught in the deck or code-demo — flag any assessment item testing untaught content.
- The assignment reinforces the same tool/technique/dataset as the code-demo, not a disconnected task.
- The hands-on guide's tool list matches what the code-demo/deck actually use that day — no tool named in the guide that the session never touches, no tool the code-demo depends on that the guide omits.
- A capstone project builds on tools/techniques the session actually taught across the code-demos, not content introduced for the first time in the project brief itself.
- Terminology and example data are consistent across deck + code-demo + MCQ + assignment + hands-on guide + project (same tool names, same running example/dataset).
- If reviewing multiple days at once: difficulty escalates coherently day-over-day.

---

## 2. Orchestration

1. Determine which artifacts are present in the bundle (not every session has all six — most won't have a project except on a capstone day). **Exclude fixture/input data** — PDFs, CSVs, or seed "knowledge base" docs a code-demo or no-code build *consumes* as demo input (e.g. `Uber-Q3-25-Earnings-Press-Release.pdf`, `prod_small.csv`, the Session 4 `Make Agent Knowledge Base.docx` used as agent seed content) are raw materials the artifact uses, not deliverables being taught — don't run any hat against them directly. Also exclude post-delivery records (filled-in gradesheets, learner response exports, project submission zips) — these are Impact-Report-stage outputs, not pre-ship content; only a *blank rubric template* is pre-ship content, and that's already covered by `assignment-review`/`project-review` §1.1.
2. Run each applicable per-type reviewer agent **first**, independently — `acceler-post-sales:deck-reviewer`, `:code-demo-reviewer`, `:mcq-reviewer`, `:assignment-reviewer`, `:hands-on-guide-reviewer`, `:project-reviewer` — each reaching its own local verdict via its own fix loop before this step continues.
3. Once all applicable per-type reviews have landed, run two bundle-level hats — order doesn't matter between them, both are blind to each per-type hat's internal reasoning or fix history, only the artifacts and final verdicts:
   - `acceler-post-sales:coherence-reviewer` (`agents/coherence-reviewer.md`) — internal consistency: do the artifacts agree with each other.
   - `acceler-post-sales:audience-fit-reviewer` (`agents/audience-fit-reviewer.md`) — external fit: does the bundle match the actual cohort (aggregated onboarding form responses) and the program's stated expectations (Discovery Facts Sheet). Different question from coherence, run it even when coherence is clean.
4. Any finding from either bundle-level hat goes through the same shared loop mechanics as the per-type skills — see `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. "The artifact" means whichever bundle artifact the fix actually touches; same guardrail applies: the fixer may only edit that artifact, never the stated objectives, the onboarding/discovery data, or any hat's rubric.

---

## 3. Risk tiering — not every session gets the same depth

*Borrowed from production code-review practice: tier by risk, not by artifact type or author. A config-file change gets a linter and a glance; a payments-path change gets the full stack. Apply the same logic here.*

- **High-stakes** (flagship/strategic client, capstone or certification-bearing content, anything public-facing or client-demo'd live): always run the full panel regardless of how clean it looks, and the human checklist read is mandatory in full — no skimming past it even on a clean ✅.
- **Standard** (routine session content, internal iteration, non-flagship cohorts): the panel still runs in full — the hats are cheap — but a clean ✅ lets the human do the fast 5-item skim from §4 rather than a deep read of every artifact.
- This is a depth-of-*human-attention* lever, not a hat-skipping lever — never drop a hat because content is "low stakes," only adjust how much the human is expected to read before signing off.

---

## 4. Final tier (PR vocabulary — a recommendation, not an auto-ship)

*"AI review is a sensor, not a verdict" — the tier below is data for the human checklist in §5, never a decision made on the human's behalf. Nothing ships without an explicit human sign-off, even on a clean ✅.*

| Tier | Condition |
|---|---|
| ✅ **Approve** | Every per-type review landed Approve or Comment-only, and both bundle-level hats (coherence, audience-fit) are Approve |
| 💬 **Comment** | Non-blocking notes remain anywhere (subjective/stylistic, or a dismissed finding) — ships, notes stay visible |
| 🔴 **Request Changes** | Any per-type review or either bundle-level hat is still Request-Changes after its fix rounds |

---

## 5. Human sign-off checklist (render verbatim, tier-labeled)

Reuses the honest-flagging convention from `requirement-mapping`'s coverage matrix rather than inventing new UI. Precede it with a short plain-language **walkthrough** (2-4 sentences: what this session's content is, what changed since the last review if any, what the tier means) so the human has orientation before triage, not just a raw findings dump. Keep the checklist itself short *because* the hats already did the breadth work — never pad it back out by re-asking what a hat already answered.

```
## Human Sign-Off — [Client] · [Program] · Day N · Tier: ✅/💬/🔴

- [ ] Matches what we promised this client/cohort for this session (curriculum/contract fit)
- [ ] Brand/tone consistent with Acceler house style
- [ ] 🔴 findings (if any): read + decide — approve with note / send back
      [list unresolved findings here, only if tier is 💬 or 🔴]
- [ ] No confidential/sensitive data present (final human eyeball)
- [ ] Sign-off: Approve / Approve with edits / Send back
```

---

## 6. Human commands (during the fix loop, not just approve/reject)

Borrowed from CodeRabbit's comment-command vocabulary — gives the human more than a binary choice on each finding:

- `explain <finding>` — get the hat's fuller reasoning/evidence before deciding
- `resolve <finding>` — dismiss it and log to that hat's §4 Memories in one step, so it won't resurface
- `re-review` — after manual edits made outside the loop, re-run just the affected hat(s) fresh rather than restarting the whole panel

---

## 7. Modes

- **Default (interactive):** surfaces findings, waits for human approval before applying any fix — matches how suggestion-based PR review tools work, never auto-applies.
- **`--report-only`:** runs every hat, returns the structured findings list, applies no fixes, asks no questions. For a fast read before deciding whether to engage the full loop.
- **Start lenient:** on first rollout, default to surfacing only high-confidence findings (the equivalent of CodeRabbit's "chill" profile) rather than maximal strictness — an assertive first pass on content with no prior review history produces an overwhelming, trust-eroding volume of comments. Tighten only once the team trusts the panel's judgment.

---

## 8. Checklist
- [ ] All applicable per-type reviews ran and reached a local verdict before either bundle-level hat ran
- [ ] Both coherence-reviewer and audience-fit-reviewer ran, not just one
- [ ] Coherence hat only checks what §1 lists — no re-litigating per-type findings
- [ ] Risk tier (§3) set before running — determines human-attention depth, never which hats run
- [ ] Final tier follows §4 exactly, framed as a recommendation, not an auto-ship
- [ ] Human checklist is the 5 items in §5 plus the walkthrough, nothing added without a stated reason
