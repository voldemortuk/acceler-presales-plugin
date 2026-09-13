---
name: instructor-finalization-skills-acceler-post-sales-day-by-day-roster
description: "Turns the pre-sales instructor shortlist logic (same Knowledge Graph, same deliverability heuristic, reused directly, not rebuilt) into a day-by-day roster against a finalized Lesson Plan. A distinct step inside acceler-post-sales, not a separate plugin, and not an independent Knowledge Graph. Designed to get better automatically once the B2C team's feedback-loop analysis connects to the same graph, no rebuild needed here when that happens."
metadata:
  type: reference
---

# Acceler Instructor Finalization · Post-Sales · SKILL.md (v1)

**What this is, and what it deliberately isn't.** This reuses the existing `acceler-presales:instructors` command's logic wholesale, the same Knowledge Graph, the same post-sales deliverable-shortlist heuristic, the same tier reference. It is not a new instructor database, not a new plugin, and not a rebuilt matching algorithm. What's actually new here is applying that logic **per day**, against a finalized Lesson Plan, instead of once per a single domain query.

---

## 1. Input

**Mandatory:** `Outputs/[Client]/lesson-plan.xlsx`, approved. Each day-tab's Topic and Subtopic columns are the query input, one instructor search per day, not one search for the whole engagement.

Runs after Lesson Plan, not before, that's the whole point, matching needs the actual day-by-day topic breakdown to be meaningful rather than matching against a vague domain.

---

## 2. Reuse, not reinvention

For each day, run the exact same query `acceler-presales:instructors` already runs: topic match against `graph.json`'s instructor nodes via `expert-in` edges, filtered through `instructor_delivery_flags.json` to exclude `pre_sales_only` names, ranked by the same deliverability heuristic (active IK instructor, dense `proposed-for` footprint, in-house staff outranking pure external marquee). Use the **post-sales deliverable shortlist** branch of that logic always, this is staffing, never the pre-sales-credibility branch.

Don't restate the tier reference table or the deliverability heuristic here, read them directly from `commands/instructors.md` and `skills/mini-ut-context` at query time.

---

## 3. Day-by-day roster, with the scheduling rule

Output one instructor per day (or per half-day, if the Lesson Plan's tabs split that way), not a single shortlist for the whole engagement.

**Consecutive-day default:** avoid assigning the same instructor to two days running. This is a soft default, not a hard block, override it only when it's genuinely needed and the instructor agrees, state explicitly whenever an override is applied and why.

---

## 3a. Checking real precedent trackers, don't conflate roles

Good instinct, worth doing: if this exact program has run before, a real internal ops tracker for that prior cohort is a stronger signal than a fresh graph query, since it names people who've actually delivered this specific content. But a real tracker usually has more than one role in it, don't take the first name found near a day number as "the instructor."

Confirmed happening on a real test run (e& AI Builder Low Code, 2026-09-13): a tracker's "NP Team Checklist" tab listed names under "Day N Development" rows, that's who *authored* the curriculum, a different job from who *taught* it live. The same tracker had a separate "Schedule & Instructor" tab with its own "SME list" column, the actual live delivery instructors, three completely different names. Before naming someone as a past instructor from a real tracker, confirm the exact column or row label they came from actually says delivery, teaching, or SME, not development, content creation, or ownership of a different task.

---

## 4. This gets better on its own, don't rebuild it later

The Knowledge Graph this reads from is already on a 12-hour dynamic sync (Drive plus instructor rating sheets). Separately, the B2C team's feedback-loop tool already produces per-instructor performance analysis and is planned to connect into the same graph. When that connection lands, instructor rankings here improve automatically, because this skill queries the graph fresh every time, it doesn't cache or hardcode a ranking. No version bump or rebuild of this skill is needed when that happens, only the graph's own data gets richer.

---

## 5. Where it gets saved

`Outputs/[Client]/instructor-roster.md`, same per-engagement folder as everything else.

---

## 6. Open, not yet decided

Once real delivery ratings exist per instructor per module (not just topic match), how much that should outrank topic match alone is still an open call, not resolved here. Query the graph as it exists today, don't invent a weighting scheme ahead of that data existing.

---

## 7. Checklist
- [ ] Lesson Plan loaded and approved before this runs, not run against a draft
- [ ] One query per day, against that day's actual Topic/Subtopic, not one query for the whole engagement
- [ ] Post-sales deliverable branch used, `pre_sales_only` names never surfaced here
- [ ] Consecutive-day default applied, any override stated explicitly with the reason
- [ ] Any name pulled from a real precedent tracker checked against its actual column/row label, development and ownership roles not presented as delivery instructors
- [ ] Tier reference and deliverability heuristic read from the existing pre-sales files, not restated or reinvented
- [ ] Saved to `Outputs/[Client]/instructor-roster.md`
