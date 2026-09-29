---
description: "POST-SESSION stakeholder report. Generate the day-N-impact-report HTML analytics page for the client sponsor / L&D stakeholder: KPIs, per-learner tier categorisation with evidence, engagement by topic, feedback breakdown, action items. Renamed 2026-09-18 from session-recap-report, it produces Outputs/[Client]/impact-report.md and hands off to impact-report-review, the old name read as a variant of the learner-facing session-recap when it's actually a different audience entirely. For the learner-facing recap, use /acceler-post-sales:session-recap instead."
argument-hint: "<day number + feedback export/chat log/transcript, or a path to them>"
---

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Generate a stakeholder-facing post-session analytics report for this brief.

## Input
```
$ARGUMENTS
```

## How to build

1. Read `skills/session-recap/SKILL.md` in full first, especially **§0** (the data pointers to ask for upfront) and **§3** (stakeholder report structure).
2. This artifact is evidence-driven. If the feedback export, chat log, or transcript aren't provided, ask for them before drafting — don't fabricate names, quotes, or ratings.
3. Confirm the tier-assignment rule with the user before applying it: Beginner/Intermediate/Advanced is assigned from what each learner actually did and asked **that specific day**, never from tenure or job title. State this in the report's own section note.
4. If a prior day's report exists in this session or repo, reuse its exact design tokens and component classes (`.tier-grid`, `.ev-grid`, `.metric-grid`, etc.) so the series stays visually consistent.
5. Follow §4 (prose style) while writing every note, quote caption, and action item.
6. Run the §5 build checklist before presenting the result.
7. Deploy the folder as a sibling under the program's existing Vercel-connected repo (e.g. `dayN-recap-report/`), matching prior days' naming exactly. The deployed site is the real artifact, per `skills/session-recap/SKILL.md`, not a local file.
8. Also save a short summary copy to `Outputs/[Client]/impact-report.md` (KPIs, tier counts, action items), per `skills/session-recap/SKILL.md` — this gives `impact-report-review` and future reviewer-learning connections a stable place to read from.
9. Hand off to `acceler-post-sales:impact-report-review` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Evidence (feedback export/chat log/transcript) provided, or the run stopped and asked rather than fabricating names/quotes/ratings
- [ ] Tier-assignment rule confirmed with the user, applied per-day not by tenure/title
- [ ] Design tokens/components matched to a prior day's report where one exists
- [ ] §4 prose style followed throughout
- [ ] Deployed as a Vercel-connected sibling folder, matching prior-day naming exactly
- [ ] Saved a summary copy to `Outputs/[Client]/impact-report.md`
- [ ] Handed to the existing `impact-report-review`, not reviewed inline here
