---
description: "PIPELINE STAGE 1 (post-sales). Score this engagement against the existing 33-question discovery checklist, right after the deal has closed, and save the resulting Discovery Facts Sheet, which pre-sales doesn't reliably do. Blocks downstream work if discovery is Thin (<50%)."
argument-hint: "<client name, plus the proposal/discovery notes if not already in this session>"
---

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Run the post-sales discovery checklist for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/discovery-checklist/SKILL.md` in full first.
2. Score using the existing `acceler-presales:discovery-checklist` tool, don't reimplement scoring. Pull forward any answer already known from the proposal or discovery calls rather than re-asking.
3. Mark money/timeline MUST items as "confirmed from proposal" where the deal being closed already resolves them, but still record the actual figures.
4. Extract the Discovery Facts Sheet per §3, verbatim where possible, with the 3-month success metric specifically checked.
5. Save the sheet to `Outputs/[Client]/discovery-facts-sheet.md`.
6. Report the verdict per §2. If <50%, state plainly that this blocks Deep Research and everything after it, don't soften it.

## Quality checklist (apply before presenting results)

- [ ] Scored against the existing 33-question checklist, not a new one
- [ ] Answers already known from pre-sales pulled forward, not re-asked
- [ ] Facts Sheet extracted verbatim where possible, 3-month success metric specifically checked
- [ ] Saved to `Outputs/[Client]/discovery-facts-sheet.md`
- [ ] Score <50% blocks the pipeline explicitly, not just noted
