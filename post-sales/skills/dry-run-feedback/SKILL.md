---
name: dry-run-feedback-skills-acceler-post-sales-rehearsal-loop
description: "Captures what came up during a session's dry run (the internal rehearsal in front of practice learners, before the real client cohort) and routes any content-relevant finding through the existing agent-loops human-gated fix loop, the same reviewer re-verifying fresh. Also keeps a plain saved record at Outputs/[Client]/dry-run-feedback.md so this data is ready once Live Tracker (still unscoped with Utkarsh) exists to consume it. Deliberately narrow, a bridge step, not the tracker itself."
metadata:
  type: reference
---

# Acceler Dry-Run Feedback · Post-Sales · SKILL.md (v1)

**What this is, and what it deliberately isn't.** After content passes `content-review` and before the real client session, the instructor runs a dry run, a rehearsal in front of an internal practice audience (staff or alumni). Whatever comes up in that rehearsal needs to reach the same fix loop a reviewer finding already goes through, not sit in someone's notes app. This skill is that bridge. It is not the Live Tracker Utkarsh has described (a combined content + ops tracker with auto-generated links), that's a separate, still-unscoped conversation. This skill only does three things: capture the notes, route content fixes through the loop that already exists, keep a clean record.

---

## 1. Inputs

**Mandatory:**
- Raw notes or observations from the dry run: what confused the practice audience, what ran long, anything that broke or read wrong. Ask for these plainly, don't invent them if they weren't captured.
- Which artifacts are in this session's bundle, from `content-review`, so each observation can be tagged to the right one.

**Best-effort:**
- A recording or transcript of the dry run, if one exists, to pull exact wording rather than paraphrase from memory.

---

## 2. Turning raw notes into findings

Convert every content-relevant observation into the same shape a reviewer hat already produces: `{finding, evidence, suggested_fix}`, tagged to the specific artifact it affects and that artifact's existing reviewer (`deck-reviewer`, `mcq-reviewer`, `code-demo-reviewer`, and so on). Nothing new is invented here, this is the exact input `content-fixer` already expects.

**Not every observation is a content fix.** Room logistics, scheduling, anything that isn't actually about the artifact itself, gets logged in the saved record (§4) but is not forced into a fix loop, there's no artifact to fix. Mark these clearly as "not a content fix" rather than stretching them into one.

---

## 3. Routing into the existing fix loop, no new process invented

Once a finding exists, it follows the identical mechanics `agent-loops/SKILL.md` §2 already defines: a human approves the finding, `acceler-post-sales:content-fixer` applies it (blind to rationale, artifact-only guardrail), and the same original per-type reviewer re-verifies fresh. This skill is only the maker of the finding, a human observing the dry run stands in for the AI hat that would normally surface it; everything downstream of "finding exists" is unchanged.

---

## 4. Where it gets saved

`Outputs/[Client]/dry-run-feedback.md`, raw notes, the structured findings list, and what got resolved for each. This is deliberately the seed data for the future connection into `agent-loops/SKILL.md` §9 (impact-report data eventually recalibrating reviewer rubrics), once Live Tracker exists to consume it properly. Capture it now in a clean, consistent place, connect it later, don't wait for the tracker to start keeping the record.

---

## 5. What this deliberately does not do yet

No auto-generated tracking links, no ops-side tracking, no dashboard. Scope stays to capture, route, record. The full Live Tracker (a single tracker spanning content and ops) is Utkarsh's own idea from his pipeline outline and remains an open conversation, see the note in `post-sales/knowledge/engagement-catalog.md`'s neighboring skills for how precedent and delivery data connect elsewhere in this pipeline. Don't expand this skill to cover ops tracking or dashboard features without that conversation happening first.

---

## 6. Checklist
- [ ] Raw dry-run notes collected before this runs, or the run stopped and asked rather than guessing
- [ ] Every content-relevant observation converted to `{finding, evidence, suggested_fix}`, tagged to its artifact and that artifact's existing reviewer
- [ ] Non-content (ops-only) observations logged separately, not forced into a fix loop
- [ ] Each content finding routed through the standard `agent-loops` human-gated loop, the same reviewer re-verifying fresh
- [ ] Saved to `Outputs/[Client]/dry-run-feedback.md`
- [ ] Scope stayed to capture, route, record, no tracker or dashboard features invented here
