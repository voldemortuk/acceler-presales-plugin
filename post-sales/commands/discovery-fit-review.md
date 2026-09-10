---
description: "PIPELINE GATE (pre-content). Score a discovery brief via the existing discovery-checklist tool and extract a Discovery Facts Sheet (audience, stated success metric, constraints) that downstream form/lesson-plan reviewers check against. Blocks the pipeline if discovery is Thin (<50%)."
argument-hint: "<path to the discovery brief/notes>"
---

Run the discovery-fit gate against this brief.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/discovery-fit-review/SKILL.md` in full first.
2. Score the brief using the existing `acceler-presales:discovery-checklist` tool — don't reimplement scoring.
3. Launch the `acceler-post-sales:discovery-fit-reviewer` agent per §2 to extract the Discovery Facts Sheet, with special attention to the 3-month-success-metric fact.
4. Report the verdict per §5. If <50%, state plainly that this blocks downstream work — don't soften it.
5. A low score or missing MUST item is a human action item, not something routed through the fix loop (§3) — only Facts Sheet extraction errors get the normal fix loop, via `acceler-post-sales:content-fixer`.

## Quality checklist (apply before presenting results)

- [ ] Score came from the existing discovery-checklist tool
- [ ] Facts Sheet extracted verbatim where possible
- [ ] The 3-month-success fact specifically checked, not just the aggregate score
- [ ] Score gaps routed to a human action item, not the fix loop
