---
description: "Generates one program Orientation deck, logistics and expectations content following the confirmed recurring 5-program structure, not teaching content. Built through the existing live-session-deck engine and tokens, not a new deck mechanism. Hands off to the existing acceler-post-sales:orientation-reviewer for the actual review pass."
argument-hint: "<client name>"
---

Generate the Orientation deck for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/orientation-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/deep-research.md` and the approved `Outputs/[Client]/lesson-plan.xlsx`. Pull instructor bios from `Outputs/[Client]/instructor-roster.md` if it already exists.
3. Build all six structural sections per §2: credibility framing, instructor introductions, program overview with stated final outcomes, a time-blocked schedule, learner expectations, and a pre-course assessment pointer where one exists. Don't force a Bloom's-verb objective section, it doesn't apply here.
4. Build through the existing deck engine/tokens (`live-session-deck`), not a new rendering mechanism.
5. Self-verify per §3: schedule is genuinely time-blocked, final-outcomes section is specific to this engagement, not boilerplate.
6. Save to `Outputs/[Client]/orientation-deck/`.
7. Hand off to `acceler-post-sales:orientation-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] All six structural sections present, none skipped
- [ ] Schedule genuinely time-blocked, final outcomes specific, not boilerplate
- [ ] Built through the existing deck engine, not a new one
- [ ] Saved to `Outputs/[Client]/orientation-deck/`
- [ ] Handed to the existing `orientation-reviewer`, not reviewed inline here
