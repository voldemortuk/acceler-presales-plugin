---
description: "Captures dry-run observations for an engagement and routes any content-relevant finding through the existing agent-loops fix loop. Also saves a plain record to Outputs/[Client]/dry-run-feedback.md. A bridge step, not the full Live Tracker."
argument-hint: "<client name> + dry-run notes/observations, or a path to them"
---

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Capture and route this engagement's dry-run feedback.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/dry-run-feedback/SKILL.md` in full first.
2. Load the mandatory input per §1: the raw dry-run notes, and which artifacts are in this session's bundle per `content-review`. If notes weren't captured, stop and ask rather than inventing them.
3. Convert each content-relevant observation into `{finding, evidence, suggested_fix}` per §2, tagged to its artifact and that artifact's existing reviewer.
4. Log any non-content (ops-only) observation separately, not as a fix-loop finding.
5. Route every content finding through the standard `agent-loops` human-gated loop per §3, human approves, `acceler-post-sales:content-fixer` applies, the same original reviewer re-verifies fresh.
6. Save the raw notes, structured findings, and resolutions to `Outputs/[Client]/dry-run-feedback.md`.
7. State plainly what this is not: not the Live Tracker, no auto-generated links, no ops tracking.

## Quality checklist (apply before presenting results)

- [ ] Raw notes loaded, or the run stopped and asked
- [ ] Every content-relevant observation converted to a properly tagged finding
- [ ] Non-content observations logged separately, not forced into the fix loop
- [ ] Each finding routed through the existing human-gated loop, no new process invented
- [ ] Saved to `Outputs/[Client]/dry-run-feedback.md`
- [ ] Scope stayed narrow, no tracker/dashboard behavior added
