---
description: "Generates one in-class hands-on build artifact for a session, a notebook, a code file, or a no-code/low-code build guide, whichever that day's lesson plan row calls for. Actually runs code before handoff to capture real output. Hands off to the existing acceler-post-sales:code-demo-reviewer for the actual review pass."
argument-hint: "<client name + which day/session this demo is for>"
---

Generate the hands-on demo for this session.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/demo-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`. If either is missing, stop and ask.
3. Determine the shape, notebook/code or no-code build guide, from that day's Libraries/Tools column, don't default to notebook.
4. Build against §2's rules for that shape, worked example before independent practice, scaffolding that fades, Bloom's level matched, no real credentials or PII.
5. If notebook or code, actually run it per §3, capture real output, don't claim it works without evidence.
6. Save to `Outputs/[Client]/demo/`.
7. Hand off to `acceler-post-sales:code-demo-reviewer` for the actual review pass, this command doesn't review its own output.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Correct shape chosen from the Libraries/Tools column, not defaulted
- [ ] Notebook/code actually executed, real output captured
- [ ] No-code steps are concrete and mechanically followable
- [ ] Worked example precedes independent practice, scaffolding fades across the demo
- [ ] No real credentials, PII, or license-incompatible code
- [ ] Saved to `Outputs/[Client]/demo/`
- [ ] Handed to the existing `code-demo-reviewer`, not reviewed inline here
